<template>
  <div class="windows-create pt-3 border border-secondary rounded p-2 mb-2">
    <i>Window ID </i>
    <input v-model="id" />
    <br>
    <i>Window Name </i>
    <input v-model="name" />
    <br>
    <i>Room ID </i>
    <input v-model="roomId" />
    <br>
    <i>Room Name </i>
    <input v-model="roomName" />
    <br>
    <i>Window Status </i>
    <input v-model="windowStatus" />
    <hr/>
    <button type="button" @click="createWindow">Create Window</button>   
    <hr/>
     {{api}} {{id}} {{name}} {{roomId}} {{roomName}} {{windowStatus}}
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {

  name: 'WindowCreate',
  
  data: function() {
    return {
      id:null,
      name: null,
      roomId: null,
      roomName: null,
      windowStatus: null
    }
  },

/*
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/windows`);
    let windows = response.data;
    this.windows = windows;
  },
  */
  methods: {
    createWindow: async function () {
       /*Find the place of window objectw ith the same Id in the array, and replace it*/ 
      /*let index = this.windows.findIndex(window => window.id === newWindow.id);
      this.windows.splice(index, 1, newWindow);*/
      await axios.post(`${API_HOST}/api/windows`, {
        id: this.id,
        name: this.name,
        roomId: this.roomId,
        roomName: this.roomName,
        windowStatus: this.windowStatus,
        })
          .then((response) => {console.log(response);}, (error) => {console.log(error);})
    }
  }
}

</script>

