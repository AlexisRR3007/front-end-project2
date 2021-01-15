<template>
  <div class="windows-create pt-3 border border-secondary rounded p-2 mb-2">
    <i>Window Name </i>
    <input v-model="name" />
    <br />
    <i>Room ID </i>
    <input v-model="roomId" />
    <br />
    <i>Window Status </i>
    <input v-model="windowStatus" />
    <hr />
    <button type="button" @click="createWindow">Create Window</button>
    <hr />
    <ul v-if="errors && errors.length">
      <li v-for="error of errors" :key="error">
        {{ error.message }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import { API_HOST } from "../config";

export default {
  name: "WindowCreate",
  props: ["windows"],

  data: function() {
    return {
      name: "",
      roomId: "",
      windowStatus: "",
      errors: [],
    };
  },

  methods: {
    async createWindow() {
      try {
        let response = await axios.post(`${API_HOST}/api/windows`, {
          name: this.name,
          roomId: this.roomId,
          windowStatus: this.windowStatus,
        });
        let updatedWindow = response.data;
        this.$emit("window-updated", updatedWindow);
      } catch (e) {
        this.errors.push(e);
      }
    },
  },
};
</script>
