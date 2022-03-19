<template>
  <aside
    :class="`fixed top-0 left-0 min-h-screen pt-16 z-40 w-full max-w-xs transform duration-300 bg-gray-800 ${
      $store.state.menu_is_active ? '' : '-translate-x-full'
    }`"
  >
    <div class="p-4">
      <h2 class="text-gray-500 text-sm font-bold uppercase mb-2">Menu</h2>

      <div class="-mx-4">
        <menu-item to="/">
          <span class="material-icons mr-2">home</span>Home
        </menu-item>

        <menu-item to="/authors">
          <span class="material-icons mr-2">group</span>Authors
        </menu-item>
      </div>
    </div>
  </aside>
</template>

<script>
import { computed } from "@vue/runtime-core";
import { useStore } from "vuex";
import MenuItem from "./MenuItem.vue";

export default {
  components: { MenuItem },
  setup() {
    const store = useStore();

    return {
      menu_is_active: computed(() => store.state.menu_is_active),
    };
  },
};
</script>
