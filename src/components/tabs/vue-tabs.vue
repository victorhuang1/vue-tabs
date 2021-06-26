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
        // 2. 过滤出我们所写的标签页
        const pageSlots = slots.filter(
          (vnode) => vnode.type && vnode.type.name == "tab-page"
        );
        //3. 获取每个标签页的 props属性
        const pages = pageSlots.map(({ props }) => props);
        if (pages) {
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
  },
};
</script>