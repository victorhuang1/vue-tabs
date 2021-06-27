<template>
  <div>
    <div style="padding: 20px;">
      <button class="button" @click="addTab">增加标签</button>
    </div>
    <vue-tabs
      v-model="currentName"
      @tab-remove="removeTab"
      style="padding: 20px;"
    >
      <tab-page label="tab 0" name="0"
        >这个标签页删不掉，因为它是静态的</tab-page
      >
      <tab-page
        v-for="item in tabsList"
        :key="item.name"
        :label="item.label"
        :name="item.name"
      >
        <b> {{ item.content }} </b>
      </tab-page>
    </vue-tabs>
  </div>
</template>

<script>
import vueTabs from "./components/packages/tabs";

import tabPage from "./components/packages/tabPage";

export default {
  name: "tab-test",

  components: {
    vueTabs,
    tabPage,
  },
  data() {
    return {
      //当前页面
      currentName: undefined,
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
        label: "New Tab ",
        name: newTabName,
        content: "New Tab content",
      };

      this.tabsList.push(data);
      // 将当前标签页切换到新增
      this.currentName = newTabName;
    },
    removeTab(targetName) {
      let tabs = this.tabsList;
      let activeName = this.currentName;
      if (activeName === targetName) {
        tabs.forEach((tab, index) => {
          if (tab.name === targetName) {
            let nextTab = tabs[index + 1] || tabs[index - 1];
            if (nextTab) {
              activeName = nextTab.name;
            }
          }
        });
      }
      this.currentName = activeName;
      // 过滤掉选择删除的标签
      this.tabsList = tabs.filter((tab) => tab.name !== targetName);
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