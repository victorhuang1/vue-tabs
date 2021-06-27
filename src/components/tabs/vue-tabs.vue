<template>
  <div>
    <!--标签栏封装-->
    <div class="tabs">
      <ul class="tabs-nav">
        <!--根据标签页个数生成按钮-->
        <li
          v-for="item in pages"
          :key="item.name"
          @click="toggleTab(item.name)"
          v-html="item.label"
          :class="classActive(item.name)"
        ></li>
      </ul>
    </div>
    <!--标签页 插槽-->
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: "vue-tabs",
  data() {
    return {
      // 当前选中的
      currentName: undefined,
      // 标签页个数
      pages: [],
    };
  },
  methods: {
    // 计算标签页个数
    calcPagesInstances() {
      // 1. 通过 $slots.default()可以获取 vue-tabs 下的全部 内容组件
      let slots = this.$slots.default();
      if (slots) {
        let pageSlots = [];
        // 2. 过滤出我们所写的标签页
        /* const pageSlots = slots.filter(
          (vnode) => vnode.type && vnode.type.name == "tab-page"
        ); */
        slots.forEach((item) => {
          if (!item.type) return;
          //使用 v-for 情况下获取子组件的方式不一样
          if (item.type.name == "tab-page") {
            pageSlots.push(item);
          } else {
            if (item.children.length > 0) {
              let temp = item.children.filter(
                (vnode) => vnode.type && vnode.type.name == "tab-page"
              );
              pageSlots = pageSlots.concat(temp);
            }
          }
        });
        /* //3. 获取每个标签页的 props属性
        const pages = pageSlots.map(({ props }) => props);
        if (pages.length>1) {
          // 4. 默认第一个页面是激活的
          this.currentName = pages[0].name;
          this.pages = pages;
        }
      }
    },
    toggleTab(name) {
      this.currentName = name;
    },
    classActive(label) {
      return this.currentName === label ? "active" : "";
    },
  },
  mounted() {
    //5. 当全部渲染完成时候运行按钮计算方法。
    this.calcPagesInstances();
  }, */
  const pagesInstanceList = pageSlots.map(({ props }) => props);
        // 判断 pages 是否有变动，避免死循环
        const pagesChanged = !(
          this.pages &&
          pagesInstanceList.length === this.pages.length &&
          pagesInstanceList.every(
            (item, index) => item.name === this.pages[index].name
          )
        );
        if (pagesChanged) {
          this.currentName = pagesInstanceList[0].name;
          this.pages = pagesInstanceList;
        }
      }
    },
    toggleTab(name) {
      this.currentName = name;
    },
    classActive(label) {
      return this.currentName === label ? "active" : "";
    },
  },
  mounted() {
    this.calcPagesInstances();
  },
  // 监听数据变化重新计算标签页
  updated() {
    this.calcPagesInstances();
  },
};
</script>