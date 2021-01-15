<template>
  <div id="app">
    <form>
      <h1>EMOM Timer</h1>
      <div class="container" v-show="settingsActive">
        <div class="group">
          <input type="text" required v-model="intervals"/>
          <span class="highlight"></span>
          <span class="bar"></span>
          <label>Rounds</label>
        </div>
        <a v-on:click="startTimer">Start</a>
      </div>
      <div v-show="!settingsActive">
        <p class="timer">{{ displayTime }}</p>
        <a v-on:click="stopTimer">Stop</a>
      </div>
      <!-- <toggle-button
        checkedElement="tp-emom"
        idLeft="tp-emom"
        textLeft="EMOM"
        idRight="tp-amrap"
        textRight="AMRAP" /> -->
    </form>
  </div>
</template>

<script>
// import ToggleButton from './components/ToggleButton.vue'
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: "App",
  components: {
    // ToggleButton
    // HelloWorld
  },
  data() {
    return {
      settingsActive: true,
      intervals: 30,
      timerCountSeconds: 30,
    };
  },
  computed: {
    displayTime: function () {

      if(this.settingsActive)
        return "Timer paused";

      var precountActive = this.timerCountSeconds > this.intervals * 60;
      var seconds = precountActive
        ? this.timerCountSeconds - this.intervals * 60
        : this.timerCountSeconds;
      var msg = new Date(seconds * 1000).toISOString().substr(11, 8);

      var rest = seconds % 60;
      var playA = rest <= 3; //msg.endsWith("03") || msg.endsWith("02") || msg.endsWith("01");
      var playB = rest == 0; //msg.endsWith("00");
      if (playB) {
        new Audio(require("./assets/start.mp3")).play();
      } else if (playA) {
        new Audio(require("./assets/beep.mp3")).play();
      }

      return msg;
    },
  },
  methods: {
    toggleSettings: function () {
      this.settingsActive = !this.settingsActive;
    },
    startTimer: function () {
      this.settingsActive = false;
      this.timerCountSeconds = 60 * this.intervals + 5; // 5 Seconds to get Ready
    },
    stopTimer: function () {
      this.settingsActive = true;
    },
  },
  watch: {
    timerCountSeconds: {
      handler(value) {
        if (!this.settingsActive && value > 0) {
          setTimeout(() => {
            this.timerCountSeconds--;
          }, 1000);
        }
      },
      immediate: true, // This ensures the watcher is triggered upon creation
    },
  },
};
</script>

<style>
@font-face {
  font-family: "PermanentMarker";
  src: local("PermanentMarker"), local("PermanentMarker-Regular"),
    url("./assets/PermanentMarker-regular.ttf") format("truetype");
  font-style: normal;
  font-weight: 400;
}

#app {
  font-family: "PermanentMarker", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 60%;
  margin: 0 auto;
}

a:hover {
  cursor: pointer;
}

.timer {
  font-size: 12em;
}

.container {
  width: 40%;
  margin: 0 auto;
}


.group 			  { 
  position:relative; 
  margin-bottom:45px; 
}
input 				{
  font-size:18px;
  padding:10px 10px 10px 5px;
  display:block;
  width:100%;
  border:none;
  border-bottom:1px solid #757575;
}
input:focus 		{ outline:none; }

/* LABEL ======================================= */
label 				 {
  color:#999; 
  font-size:18px;
  font-weight:normal;
  position:absolute;
  pointer-events:none;
  left:5px;
  top:10px;
  transition:0.2s ease all; 
  -moz-transition:0.2s ease all; 
  -webkit-transition:0.2s ease all;
}

/* active state */
input:focus ~ label, input:valid ~ label 		{
  top:-20px;
  font-size:14px;
  color:#5264AE;
}

/* BOTTOM BARS ================================= */
.bar 	{ position:relative; display:block; width:100%; }
.bar:before, .bar:after 	{
  content:'';
  height:2px; 
  width:0;
  bottom:1px; 
  position:absolute;
  background:#5264AE; 
  transition:0.2s ease all; 
  -moz-transition:0.2s ease all; 
  -webkit-transition:0.2s ease all;
}
.bar:before {
  left:50%;
}
.bar:after {
  right:50%; 
}

/* active state */
input:focus ~ .bar:before, input:focus ~ .bar:after {
  width:50%;
}

/* HIGHLIGHTER ================================== */
.highlight {
  position:absolute;
  height:60%; 
  width:100px; 
  top:25%; 
  left:0;
  pointer-events:none;
  opacity:0.5;
}

/* active state */
input:focus ~ .highlight {
  -webkit-animation:inputHighlighter 0.3s ease;
  -moz-animation:inputHighlighter 0.3s ease;
  animation:inputHighlighter 0.3s ease;
}

/* ANIMATIONS ================ */
@-webkit-keyframes inputHighlighter {
	from { background:#868686; }
  to 	{ width:0; background:transparent; }
}
@-moz-keyframes inputHighlighter {
	from { background:#5c5c5c; }
  to 	{ width:0; background:transparent; }
}
@keyframes inputHighlighter {
	from { background:#1d1d1d; }
  to 	{ width:0; background:transparent; }
}
</style>
