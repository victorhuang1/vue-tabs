<script>
import {
  ref,
  getCurrentInstance,
  onMounted,
  onUpdated,
  h,
  Fragment,
  renderList,
  provide,
  watch,
} from "vue";
export default {
  name: "vue-tabs",
  props: {
    // 当前选中的标签页
    modelValue: {
      type: String,
      default: "",
    },
  },
  // 这个组件会往上抛的事件，必须写明
  emits: ["input", "update:modelValue", "edit", "tab-remove"],
  setup(props, ctx) {
    const nav$ = ref(null);
    const currentName = ref(props.modelValue || "0");
    const pages = ref([]);

    // 实例对象
    const instancs = getCurrentInstance();
    // 用来管理 标签页状态 一个简单的 store 模式
    const pageStateMap = {};
    // 提供子组件 inject 获取
    provide("rootTabs", {
      props,
      currentName,
    });

    /**
     * 3x 取消了 $on 事件
     * 3x 下 `vnode` 的参数结构也发生改变，没办法直接获取 组件的 data 及 computed.
     * 这里采用 provide 与 inject 组成回调函数来同步状态变化。
     * $on、$emit 本质上也是使用回调函数实现的观察者模式。
     */
    provide("updatePageStateCallback", (page) => {
      pageStateMap[page.uid] = page;
    });

    // 监听 v-model的变动
    watch(
      () => props.modelValue,
      (modelValue) => {
        setCurrentName(modelValue);
      }
    );

    // 处理标签页点击
    const handleTabClick = (tabName) => {
      setCurrentName(tabName);
    };
    // 标签删除
    const handleTabRemove = (tabName, event) => {
      // 阻止冒泡事件
      event.stopPropagation();
      ctx.emit("edit", tabName, "remove");
      ctx.emit("tab-remove", tabName);
    };
    // 切换当前标签页
    const setCurrentName = (value) => {
      currentName.value = value;
      // 告诉 v-model 改变
      ctx.emit("input", value);
      ctx.emit("update:modelValue", value);
    };

    // 过滤出 标签页
    const getPageVnodeFromSlost = (vnode = [], componentInstances = []) => {
      Array.from(vnode.children || []).forEach((node) => {
        let type = node.type;
        type = type.name || type;
        if (type === "tab-page" && node.component) {
          componentInstances.push(node.component);
          // $slots的节点是获取不到 name的, 可以使用 fragment 进行判断
        } else if (type === Fragment) {
          getPageVnodeFromSlost(node, componentInstances);
        }
      });
      return componentInstances;
    };

    /**
     * 之前我们使用这个方法计算 page 的个数，
     */
    const calcPagesInstances = () => {
      if (ctx.slots.default()) {
        const children = instancs.subTree.children;
        //
        const content = Array.from(children).find(({ type }) => {
          return type === Fragment;
        });
        if (!content) return;
        /**
         * 1. 从 content 过滤出 tab-page 的vnode.
         * 2. 之前我们定义了 page 的store - pageStateMap, 它的数据是通过子组件回调注册的。
         * 3. 如果是静态的下面的就完成了，但是我们要实现增删，
         *    `updatePageStateCallback` 只实现了注册，如果在删除的时候顺便删除它，就破坏了结构增加了维护难度。
         *    所以这里通过老办法计算我们真是的标签去过滤那些没删除的数据，实现动态增删
         */

        // let pageVnodes = [];
        // for (let p in pageStateMap) {
        //   pageVnodes.push(pageStateMap[p]);
        // }
        const pageVnodes = getPageVnodeFromSlost(content).map((item) => {
          return pageStateMap[item.uid];
        });

        // 通过 page 的uid 判断是否有变动，避免死循环
        const pagesChanged = !(
          pageVnodes.length === pages.value.length &&
          pageVnodes.every((item, index) => item.uid === pages.value[index].uid)
        );
        if (pagesChanged) {
          pages.value = pageVnodes;
        }
      } else if (pages.value.length !== 0) {
        //如果没有 slot 则强制初始化
        pages.value = [];
      }
    };

    // 页面渲染后计算标签页个数
    onMounted(() => {
      calcPagesInstances();
    });

    // 页面变动的时候重新计算标签页个数
    onUpdated(() => {
      calcPagesInstances();
    });

    return {
      nav$,
      currentName,
      pages,
      handleTabClick,
      handleTabRemove,
    };
  },
  // 3x 下的render 不自带 createElement 方法
  render() {
    let { pages, handleTabClick, handleTabRemove } = this;

    const el_tabs_buttons = renderList(pages, (item, index) => {
      // 标签名
      const tabName = item.props.name || item.index || `${index}`;
      // 索引
      item.index = `${index}`;
      //
      const tabLabel = item.props.label;

      const tabindex = item.active ? 0 : -1;
      //  这里的逻辑 类似 v-if，所以最好带上 key
      const btnClose = h(
        "span",
        {
          onClick: (ev) => {
            handleTabRemove(tabName, ev);
          },
        },
        ["x"]
      );

      return (
        <li
          id={`tab-${tabName}`}
          key={`tab-${tabName}`}
          class={{ active: item.active }}
          ref="tabs"
          tabIndex={tabindex}
          onclick={() => handleTabClick(tabName)}
        >
          {tabLabel} {btnClose}
        </li>
      );
    });

    // 标签栏
    const el_tabs_nav = (
      <div class="tabs">
        <ul class="tabs-nav">{el_tabs_buttons}</ul>
      </div>
    );

    return h("div", [el_tabs_nav, this.$slots?.default()]);
  },
};
</script>