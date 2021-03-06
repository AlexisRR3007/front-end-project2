<template>
  <div
    class="window border border-secondary rounded p-2 mb-2"
    :class="{ expanded: isExpanded }"
  >
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="window-name fw-bold pe-3">{{ window.name }}</div>
      <div class="room-name text-muted">{{ window.room.name }}</div>

      <div
        class="open-status ms-4"
        :class="{
          open: isWindowOpen && !windowIsSwitchingIcon,
          closed: !isWindowOpen && !windowIsSwitchingIcon,
        }"
      >
        <template v-if="isWindowOpen && !windowIsSwitchingIcon">
          <span class="icon">&#x2B24;</span> Open
        </template>
        <template v-if="!isWindowOpen && !windowIsSwitchingIcon">
          <span class="icon">&#x2716;</span> Closed
        </template>
        <template v-if="isWindowOpen && windowIsSwitchingIcon">
          <span class="icon">&#9203;</span> Wait for closing
        </template>
        <template v-if="!isWindowOpen && windowIsSwitchingIcon">
          <span class="icon">&#9203;</span> Wait for opening
        </template>
      </div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? "&#9660;" : "&#9658;" }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr />
      <div class="details d-flex">
        <button
          type="button"
          class="btn btn-primary me-2"
          :class="{ disabled: windowIsSwitching }"
          @click="switchWindow"
        >
          {{ isWindowOpen ? "Close" : "Open" }} window
        </button>
        <button
          type="button"
          class="btn btn-danger"
          :class="{ disabled: windowIsDeleting }"
          @click="deleteWindow"
        >
          Delete window
        </button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from "axios";
import { API_HOST } from "../config";

export default {
  name: "WindowsListItem",
  props: ["window"],
  data: function() {
    return {
      isExpanded: false,
      windowIsSwitching: false,
      windowIsSwitchingIcon: false,
      windowIsDeleting: false,
    };
  },
  computed: {
    isWindowOpen: function() {
      return this.window.windowStatus === "OPEN";
    },
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchWindow() {
      this.windowIsSwitching = true;
      this.isExpanded ? this.toggleExpand() : null;
      var icon = setTimeout(
        () =>
          (this.windowIsSwitchingIcon = this.windowIsSwitching ? true : false),
        300
      );
      let response = await axios.put(
        `${API_HOST}/api/windows/${this.window.id}/switch`
      );
      let updatedWindow = response.data;
      this.$emit("window-updated", updatedWindow);
      clearTimeout(icon);
      this.windowIsSwitchingIcon = false;
      this.windowIsSwitching = false;
    },
    async deleteWindow() {
      this.windowIsDeleting = true;
      this.isExpanded ? this.toggleExpand() : null;
      await axios.delete(`${API_HOST}/api/windows/${this.window.id}`);
      this.$emit("window-deleted", this.window.id);
      this.windowIsdeleting = false;
    },
  },
};
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

.window {
  background-color: white;
  .top-row {
    cursor: pointer;
  }
}
</style>
