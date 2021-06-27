<template>
  <!-- 通过 v-show 控制显示和隐藏 -->
  <div class="tabs-content" v-show="active">
    <slot></slot>
  </div>
</template>

<script>
import { ref, computed, inject, getCurrentInstance } from "vue";
export default {
  name: "tab-page",
  props: {
    //选项卡标题
    label: String,
    // 与选项卡绑定值 value 对应的标识符，表示选项卡别名
    name: String,
  },
  setup(props) {
    const index = ref(null);
    // 父组件使用 provide 提供的参数
    const rootTabs = inject("rootTabs");
    const updatePageState = inject("updatePageStateCallback");

    const active = computed(() => {
      const active = rootTabs.currentName.value === (props.name || index.value);
      return active;
    });

    const paneName = computed(() => {
      return props.name || index.value;
    });
    const instance = getCurrentInstance();
    // 通过updatePageStateCallback 实现 $on 方式同步数据
    updatePageState({
      uid: instance.uid,
      props: props,
      paneName,
      active,
      index,
    });
    return {
      index,
      active,
      paneName,
    };
  },
};
</script>