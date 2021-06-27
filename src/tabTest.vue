<template>
  <div>
    <!-- <vue-tabs>
      <tab-page label="用户管理" name="first">
        <b> 模板一 </b>
      </tab-page>

      <tab-page label="配置管理" name="second">
        <b> 模板二 </b>
      </tab-page>

      <tab-page label="角色管理" name="third">
        <b> 模板三 </b>
      </tab-page>
    </vue-tabs> -->
    <div>
      <button class="button" @click="addTab">增加标签</button>
      <button class="button">删除标签</button>
    </div>
    <vue-tabs>
      <tab-page
        v-for="item in tabsList"
        :key="item.name"
        :label="item.label"
        :name="item.name"
      ><b>{{item.content}}</b>
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
  /* props: {}, */
  data() {
    return {
      tabsList: [
        { label: "tab 1", name: "1", content: "模板1" },

        { label: "tab 2", name: "2", content: "模板2" },

        { label: "tab 3", name: "3", content: "模板3" },
      ],
      currentTab: "1",
    };
  },
  methods: {
    addTab() {
      let newTabName = this.tabsList.length + 1 + "";
      let data = {
        label: "tab 3",
        name: newTabName,
        content: "New Tab content",
      };

      this.tabsList.push(data);
    },
  },
};
</script>

<style scoped>
.button {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  border: 1px solid #dcdfe6;
  color: #606266;
  -webkit-appearance: none;
  text-align: center;
  box-sizing: border-box;
  outline: none;
  margin: 0;
  transition: 0.1s;
  font-weight: 500;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  padding: 12px 20px;
  font-size: 14px;
  border-radius: 4px;
}

.button:hover {
  background-color: #6eb4fc;
  color: #fff;
}
</style>