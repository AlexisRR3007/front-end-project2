<template>
  <div id="app">
    <header>
      <img src="./assets/logo.png" class='image' width="117" height="47">
      <div class="title">{{title}}</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <main-navigation
        :navTable="navTable"
        :navSelected="navSelected"
        @navItem-updated="updatedNavItem"></main-navigation>

      <window-create v-if="isSelected('Windows')"></window-create>
      <windows-list v-if="isSelected('Windows')"></windows-list>
      <rooms-list v-if="isSelected('Rooms')"></rooms-list>

    </section>
  </div>
</template>

<script>
import MainNavigation from './components/MainNavigation.vue';
import WindowsList from './components/WindowsList.vue';
import RoomsList from './components/RoomsList.vue';
import WindowCreate from './components/WindowCreate.vue';

export default {
  name: 'App',
  components: {
    MainNavigation,
    WindowsList,
    RoomsList,
    WindowCreate
  },
  data: function() {
    return {
      title: 'Faircorp',
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
  background-color: #F7F7F7;
  margin-top: 5px;
}

.title {
  margin-left: 5px;
  font-size: 35px;
  font-family:fantasy;
  font-style: bold;
  color: #000000;
}

</style>
