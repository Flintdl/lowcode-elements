<template>
  <div id="app">
    <MenuTemplate />
    <router-view />
  </div>
</template>

<script>
// @ is an alias to /src
import MenuTemplate from "@/components/Menu/MenuTemplate.vue";

export default {
  name: "app",
  components: {
    MenuTemplate,
  },
};
</script>
