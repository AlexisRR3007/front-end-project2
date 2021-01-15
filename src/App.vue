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
      <rooms-list v-if="isSelected('Rooms')"></rooms-list>
      <windows-list v-if="isSelected('Windows')"></windows-list>
      <heaters-list v-if="isSelected('Heaters')"></heaters-list>

    </section>
  </div>
</template>

<script>
import MainNavigation from './components/MainNavigation.vue';
import WindowsList from './components/WindowsList.vue';
import HeatersList from './components/HeatersList.vue';
import RoomsList from './components/RoomsList.vue';

export default {
  name: 'App',
  components: {
    MainNavigation,
    WindowsList,
    HeatersList,
    RoomsList
  },
  data: function() {
    return {
      title: 'Faircorp',
      navTable: ['Rooms','Windows','Heaters'],
      navSelected: 'Rooms'
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
