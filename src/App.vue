<template>
  <div class="bg-gray-700 min-h-screen text-white">
    <header-nav />
    <sidebar-nav />
    <router-view />
  </div>
</template>

<script>
import HeaderNav from "./components/HeaderNav.vue";
import SidebarNav from "./components/SidebarNav.vue";

export default {
  name: "App",
  components: { HeaderNav, SidebarNav },
  setup() {},
};
</script>

<style></style>
