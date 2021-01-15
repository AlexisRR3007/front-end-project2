<template>
  <div class="heaters-list pt-3">
       <heater-create   
      @heater-updated="updateHeater"></heater-create>     
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


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import HeatersListItem from './HeatersListItem';
import HeaterCreate from './HeaterCreate.vue';

export default {
  components: {
    HeatersListItem,
    HeaterCreate
  },
  name: 'HeatersList',
  data: function() {
    return {
      /* Initialize heaters with an empty array, while waiting for actual data to be retrieved from the API */
      heaters: []
    }
  },

  created: async function() {
    let response = await axios.get(`${API_HOST}/api/heaters`);
    let heaters = response.data;
    this.heaters = heaters;
  },

  methods: {
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
