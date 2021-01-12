<template>
  <div class="room border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex align-items-center" @click="toggleExpand">
      <div class="room-name fw-bold pe-3">Windows</div>

      <button type="button" 
          class="btn btn-primary ms-auto me-4 btn-sm" 
          :class="{disabled: windowsAreSwitching}"
          @click.stop="switchAllWindows" > Switch all windows</button>

      <div class="expand-button">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>

    <template v-if="isExpanded">
      <hr/>
       <div class="windows-list pt-3 d-flex flex-column">
          <windows-list-item 
            v-for="window in windows"
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
  props: ['windows',"roomId"],
  components: {
    WindowsListItem
  },
  data: function() {
    return {
      isExpanded: false,
      windowsAreSwitching : false
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchAllWindows() {
      this.windowsAreSwitching = true;
      await axios.put(`${API_HOST}/api/rooms/${this.roomId}/switchWindow`);
      for(let i = 0; i < this.windows.length; i++) {
        this.windows[i].windowStatus = this.windows[i].windowStatus==='OPEN' ? 'CLOSED' : 'OPEN';
      }
      this.windowsAreSwitching = false;
    },
    updateWindow(newWindow) {
      /* Find the place of window objectw ith the same Id in the array, and replace it */
      let index = this.windows.findIndex(window => window.id === newWindow.id);
      this.windows.splice(index, 1, newWindow);
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
  .top-row {
    cursor: pointer;
  }
}

.sub-title {
  margin: 15px;
}
</style>
