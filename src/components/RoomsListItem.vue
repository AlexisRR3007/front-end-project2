<template>
  <div class="room border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="room-name fw-bold pe-3">{{room.name}}</div>
      <div class="room-name text-muted">Floor {{room.floor.floorNumber}}</div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>

    <template v-if="isExpanded">
      <hr/>
       <div class="windows-list pt-3 d-flex flex-column">
          <rooms-windows-list 
            :roomId="room.id"
            :windows="room.listOfWindows"
          >
          </rooms-windows-list>
          </div>
    </template>

  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomsWindowsList from './RoomsWindowsList';

export default {
  name: 'RoomsListItem',
  props: ['room'],
  components: {
    RoomsWindowsList
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
  background-color: white;
  .top-row {
    cursor: pointer;
  }
}
</style>
