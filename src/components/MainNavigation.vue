<template>
  <ul class="nav nav-tabs">
    <li
      class="nav-item"
      v-for="navItem in navTable"
      :key="navItem"
      @click="switchNavItem(navItem)"
    >
      <a
        class="nav-link"
        :class="{ active: isSelected(navItem) }"
        style="color: black"
        href="#"
        :aria-current="isSelected(navItem) ? 'page' : false"
      >
        {{ navItem }}</a
      >
    </li>
  </ul>
</template>

<script>
export default {
  name: "MainNavigation",
  props: ["navTable", "navSelected"],
  methods: {
    isSelected: function(navItem) {
      return this.navSelected == navItem;
    },
    switchNavItem: function(navItem) {
      this.$emit("navItem-updated", navItem);
    },
  },
};
</script>
