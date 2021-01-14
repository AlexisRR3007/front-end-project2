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
     <ul v-if="errors && errors.length">
      <li v-for="error of errors">
        {{error.message}}
      </li>
    </ul>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {

  name: 'WindowCreate',
  props: ['windows'],
  
  data: function() {
    return {
      id:-"",
      name: "",
      roomId: "",
      roomName: "",
      windowStatus: "",
      errors: []
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
   async createWindow() {
      try {
        await axios.post(`${API_HOST}/api/windows`, {
        id: this.id,
        name: this.name,
        roomId: this.roomId,
        roomName: this.roomName,
        windowStatus: this.windowStatus
        })
      } catch (e) {
        this.errors.push(e)
      }
    } 
  }
}

</script>

=======
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

  methods: {
    createWindow: async function () {
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

