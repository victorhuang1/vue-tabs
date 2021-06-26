<template>
  <!-- 通过 v-show 控制显示和隐藏 -->
  <div class="tabs-content" v-show="isActive">
    <slot></slot>
  </div>
</template>

<script>
export default {
  inject: [],
  name: "tab-page",
  props: {
    //选项卡标题
    label: {
      type: String,
      default: "",
    },
    // 与选项卡绑定值 value 对应的标识符，表示选项卡别名
    name: {
      type: String,
      default: "",
    },
  },
  computed: {
    isActive() {
      return this.name === this.$parent.currentName ? true : false;
    },
  },
};
</script>