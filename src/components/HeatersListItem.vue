<template>
  <div class="heater border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="heater-name fw-bold pe-3">{{heater.name}}</div>
      <div class="room-name text-muted">{{heater.room.name}}</div>

      <div class="on-status ms-4" :class="{on: isHeaterOn && !heaterIsSwitchingIcon, off: !isHeaterOn && !heaterIsSwitchingIcon}">
        <template v-if="isHeaterOn && !heaterIsSwitchingIcon">
          <span class="icon">&#x2B24;</span> On
        </template>
        <template v-if="!isHeaterOn && !heaterIsSwitchingIcon">
          <span class="icon">&#x2716;</span> Off
        </template>
        <template v-if="isHeaterOn && heaterIsSwitchingIcon">
          <span class="icon">&#9203;</span> Wait for closing
        </template>
        <template v-if="!isHeaterOn && heaterIsSwitchingIcon">
          <span class="icon">&#9203;</span> Wait for oning
        </template>
      </div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <button 
          type="button" 
          class="btn btn-primary me-2"
          :class="{disabled: heaterIsSwitching}" 
          @click="switchHeater">{{ isHeaterOn ? 'Off' : 'On' }} heater</button>
        <button 
        type="button" 
        class="btn btn-danger"
        :class="{disabled: heaterIsDeleting}" 
          @click="deleteHeater">Delete heater</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'HeatersListItem',
  props: ['heater'],
  data: function() {
    return {
      isExpanded: false,
      heaterIsSwitching: false,
      heaterIsSwitchingIcon: false,
      heaterIsDeleting: false,
    }
  }, 
  computed: {
    isHeaterOn: function() {
      return this.heater.heaterStatus === 'ON'; 
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchHeater() {
      this.heaterIsSwitching = true;
      this.isExpanded ? this.toggleExpand() : null;
      var icon = setTimeout(() => this.heaterIsSwitchingIcon = this.heaterIsSwitching ? true : false, 300)
      let response = await axios.put(`${API_HOST}/api/heaters/${this.heater.id}/switch`);
      let updatedHeater = response.data;
      this.$emit('heater-updated', updatedHeater);
      clearTimeout(icon);
      this.heaterIsSwitchingIcon = false;
      this.heaterIsSwitching = false;
    },
    async deleteHeater() {
      this.heaterIsDeleting = true;
      this.isExpanded ? this.toggleExpand() : null;
      await axios.delete(`${API_HOST}/api/heaters/${this.heater.id}`);
      this.$emit('heater-deleted',this.heater.id);
      this.heaterIsdeleting = false;
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

.heater {
  background-color: white;
  .top-row {
    cursor: pointer;
  }
}
</style>
