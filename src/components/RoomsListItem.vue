<template>
  <div class="room border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="room-name fw-bold pe-3">{{room.name}}</div>
      <div class="room-name text-muted">Floor {{room.floor}}</div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>

    <template v-if="isExpanded">
      <hr/>
       <div class="windows-list pt-3 d-flex flex-column">
          <div class="fw-bold align-self-start pb-3 pe-3">Windows :</div>
          <windows-list-item 
            v-for="window in room.listOfWindow"
            :window="window"
            :key="window.id"
            @window-updated="updateWindow"
          >
          </windows-list-item>
  </div>
    </template>

  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
import WindowsListItem from './WindowsListItem';

export default {
  name: 'RoomsListItem',
  props: ['room'],
  components: {
    WindowsListItem
  },
  data: function() {
    return {
      isExpanded: false
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    updateWindow(newWindow) {
      /* Find the place of window objectw ith the same Id in the array, and replace it */
      let index = this.room.listOfWindow.findIndex(window => window.id === newWindow.id);
      this.room.listOfWindow.splice(index, 1, newWindow);
    },
    async switchRoom() {
      let response = await axios.put(`${API_HOST}/api/rooms/${this.room.id}/switch`);
      let updatedRoom = response.data;
      this.$emit('room-updated', updatedRoom);
    }
  }
}
</script>

<style lang="scss" scoped>

.open-status {
  .icon {
    position: relative;
  }

  &.open {
    color: #198754;
    .icon {
      font-size: 12px;
      top: -3px;
    }
  }

  &.closed {
    color: #dc3545;
  }
}

.room {
  .top-row {
    cursor: pointer;
  }
}

.sub-title {
  margin: 15px;
}
</style>
