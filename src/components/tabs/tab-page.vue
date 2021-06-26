<template>
  <div class="tabs-content" v-show="isActive">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "tab-page",
  props: {
    // 1、props
    // 当前标签组的 key
    current: {
      type: String,
      default: "",
    },
    // 该标签页的key
    name: {
      type: String,
      default: "",
    },
    },
    computed: {
    //   通过初始的名称，和当前的名称比较判断自己是否要显示
    isActive() {
      return this.name === this.current ? true : false;
    },
  },
};
</script>