<template>
  <div class="heaters-create pt-3 border border-secondary rounded p-2 mb-2">
    <i>Heater Name </i>
    <input v-model="name" />
    <br>
    <i>Room ID </i>
    <input v-model="roomId" />
    <br>
    <i>Heater Status </i>
    <input v-model="heaterStatus" />
    <hr/>
    <button type="button" @click="createHeater">Create Heater</button>   
    <hr/>
     <ul v-if="errors && errors.length">
      <li v-for="error of errors"
        :key="error">
        {{error.message}}
      </li>
    </ul>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {

  name: 'HeaterCreate',
  props: ['heaters'],
  
  data: function() {
    return {
      name: "",
      roomId: "",
      heaterStatus: "",
      errors: []
    }
  },

  methods: {
   async createHeater() {
      try {
       let response = await axios.post(`${API_HOST}/api/heaters`, {
  "name": this.name,
  "roomId": this.roomId,
  "heaterStatus": this.heaterStatus
  });
        let updatedHeater = response.data;
      this.$emit('heater-updated', updatedHeater);
      } catch (e) {
        this.errors.push(e)
      }
    } 
  }
}

</script>
