<template>
  <div class="timer">
    <div class="switch-field">
      <input
        type="radio"
        v-bind:id="idLeft"
        v-bind:name="name"
        v-bind:value="textLeft"
        checked
        v-model="checkedElement"
      />
      <label v-bind:for="idLeft">{{ textLeft }}</label>
      <input
        type="radio"
        v-bind:id="idRight"
        v-bind:name="name"
        v-bind:value="textRight"
        v-model="checkedElement"
      />
      <label v-bind:for="idRight">{{ textRight }}</label>
    </div>
    <!-- Config for EMOM-Timer -->
    <div v-if="checkedElement === 'EMOM'">emom</div>

    <!-- Config for AMRAP-Timer -->
    <div v-else-if="checkedElement === 'AMRAP'">
      <!-- Config -->

      <!-- Timer -->
      <label for="amrapDurationMinutes">Duration (Minutes)</label>
      <input id="amrapDurationMinutes" v-model="amrapTimer.durationMinutes" />
      <input
        type="button"
        v-bind:value="amrapTimerButtonText"
        v-on:click="toggleAmrapTimer"
      />

      <div>
        <p>{{ timerCount }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ToggleButton",
  data: function () {
    return {
      checkedElement: "EMOM",
      amrapTimer: {
        durationMinutes: 10,
        timerRunning: false,
      },
      timerCount: 30,
    };
  },
  computed: {
    amrapTimerButtonText() {
      return this.amrapTimer.timerRunning ? "Stop" : "Start";
    },
    amrapTimerText() {
      return this.amrapTimer.durationMinutes;
    },
    timerRunning() {
      return this.checkedElement === "AMRAP" && this.amrapTimer.timerRunning;
    },
  },
  methods: {
    toggleAmrapTimer() {
      this.amrapTimer.timerRunning = !this.amrapTimer.timerRunning;
      if (!this.amrapTimer.timerRunning) {
        this.timerCount--;
      }
    },
  },
  watch: {
    timerCount: {
      handler(value) {
        if (this.timerRunning && value > 0) {
          setTimeout(() => {
            this.timerCount--;
          }, 1000);
        }
      },
      //   immediate: true, // This ensures the watcher is triggered upon creation
    },
  },
  props: ["name", "textLeft", "idLeft", "textRight", "idRight"],
};
</script>

<style scoped>
.switch-field {
  display: flex;
  margin-bottom: 36px;
  overflow: hidden;
}

.switch-field input {
  position: absolute !important;
  clip: rect(0, 0, 0, 0);
  height: 1px;
  width: 1px;
  border: 0;
  overflow: hidden;
}

.switch-field label {
  background-color: #e4e4e4;
  color: rgba(0, 0, 0, 0.6);
  font-size: 14px;
  line-height: 1;
  text-align: center;
  padding: 8px 16px;
  margin-right: -1px;
  border: 1px solid rgba(0, 0, 0, 0.2);
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.3), 0 1px rgba(255, 255, 255, 0.1);
  transition: all 0.1s ease-in-out;
}

.switch-field label:hover {
  cursor: pointer;
}

.switch-field input:checked + label {
  background-color: #a5dc86;
  box-shadow: none;
}

.switch-field label:first-of-type {
  border-radius: 4px 0 0 4px;
}

.switch-field label:last-of-type {
  border-radius: 0 4px 4px 0;
}
</style>