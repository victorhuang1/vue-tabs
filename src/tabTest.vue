<template>
  <div>
    <vue-tabs>
      <tab-page label="用户管理" name="first">
        <b> 模板一 </b>
      </tab-page>

      <tab-page label="配置管理" name="second">
        <b> 模板二 </b>
      </tab-page>

      <tab-page label="角色管理" name="third">
        <b> 模板三 </b>
      </tab-page>
    </vue-tabs>
  </div>
</template>

<script>
import vueTabs from "./components/tabs/vue-tabs";
import tabPage from "./components/tabs/tab-page";

export default {
  name: "tab-test",
  components: {
    vueTabs,
    tabPage,
  },
  props: {},
};
</script>