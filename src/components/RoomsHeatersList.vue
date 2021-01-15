<template>
  <div class="room border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex align-items-center" @click="toggleExpand">
      <div class="room-name fw-bold pe-3">Heatfers</div>

      <button type="button" 
          class="btn btn-primary ms-auto me-4 btn-sm" 
          :class="{disabled: heatersAreSwitching}"
          @click.stop="switchAllHeaters" > Switch all heaters</button>

      <div class="expand-button">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>

    <template v-if="isExpanded">
      <hr/>
       <div class="heaters-list pt-3 d-flex flex-column">
          <heaters-list-item 
            v-for="heater in heaters"
            :heater="heater"
            :key="heater.id"
            @heater-updated="updateHeater"
            @heater-deleted="deleteHeater"
          >
          </heaters-list-item>
  </div>
    </template>

  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
import HeatersListItem from './HeatersListItem';

export default {
  name: 'RoomsHeatersList',
  props: ['heaters',"roomId"],
  components: {
    HeatersListItem
  },
  data: function() {
    return {
      isExpanded: false,
      heatersAreSwitching : false
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchAllHeaters() {
      this.heatersAreSwitching = true;
      await axios.put(`${API_HOST}/api/rooms/${this.roomId}/switchHeater`);
      for(let i = 0; i < this.heaters.length; i++) {
        this.heaters[i].heaterStatus = this.heaters[i].heaterStatus==='ON' ? 'OFF' : 'ON';
      }
      this.heatersAreSwitching = false;
    },
    updateHeater(newHeater) {
      /* Find the place of heater objectw ith the same Id in the array, and replace it */
      let index = this.heaters.findIndex(heater => heater.id === newHeater.id);
      this.heaters.splice(index, 1, newHeater);
    },
    deleteHeater(heaterId) {
      let index = this.heaters.findIndex(heater => heater.id === heaterId);
      this.heaters.splice(index, 1);
    }
  }
}
</script>

<style lang="scss" scoped>

.on-status {
  .icon {
    position: relative;
  }

  &.on {
    color: #198754;
    .icon {
      font-size: 12px;
      top: -3px;
    }
  }

  &.off {
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
