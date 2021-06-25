<template>
  <div id="app">
    <tabs-index></tabs-index>
  </div>
</template>

<script>
  import tabsIndex from "./components/tabs";

  export default {
    name: "App",
    components: {
      tabsIndex,
    },
  };
</script>