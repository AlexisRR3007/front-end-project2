<template>
  <div id="app">
    <header>
      <div class="title">{{title}}</div>
    </header>

    <section class="main-content w-50 mx-auto">

      <main-navigation
        :navTable="navTable"
        :navSelected="navSelected"
        @navItem-updated="updatedNavItem"></main-navigation>

      <windows-list v-if="isSelected('Windows')"></windows-list>

    </section>
  </div>
</template>

<script>
import MainNavigation from './components/MainNavigation.vue';
import WindowsList from './components/WindowsList.vue';

export default {
  name: 'App',
  components: {
    MainNavigation,
    WindowsList
  },
  data: function() {
    return {
      title: 'My App Title',
      navTable: ['Windows','Rooms'],
      navSelected: 'Windows'
    }
  },
  methods: {
    updatedNavItem: function(navItem) {
      this.navSelected = navItem;
    },
    isSelected: function (navItem) {
      return this.navSelected == navItem;
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
