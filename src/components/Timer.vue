<template>
    <h2 :class="{'white': running}">{{ formattedTime }}</h2>
    <div :class="{ 'divider': true, 'divider-white': running }"></div>
    <button @click="toggle" :class="timerIcon"></button>
    <button @click="reset" :class="{ 'reset-icon': true, 'reset-icon-white': running }"></button>
</template>

<style>
  @import url(//db.onlinewebfonts.com/c/07a38bbad54db72a40b406bed1c72f53?family=Gotham+Pro);
  body {
    background-color:#353638;
  }

  h2 {
    margin-top: 22px;
    font-family: 'Gotham Pro';
    font-style: normal;
    font-weight: 400;
    font-size: 22px;
    line-height: 21px;
    text-align: center;
    color: #9E9E9E;
    position: absolute; 
    top: 0;
    width: 100%; 
  }
  .white {
    color: #ffffff;
  }
  .divider {
    width: 225px;
    height: 2px;
    background-color: #9E9E9E;
    position: absolute;
    top: 60px;
    margin-left: 0px;
  }
  .divider-white {
    background-color: #FFFFFF;
  }
  .timer-icon {
    top: 0;
    margin-top: 80px;
    padding: 0px;
    width: 0;
    height: 0;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    position: absolute; 
    background-color: #696969;
    box-sizing: border-box;
    margin-left: -50px;
  }

  .timer-icon.running {
    width: 20px;
    height: 20px;
    border-style: double;
    border-width: 0px 0px 0px 10px;
    border-color: #FFFFFF;
    margin-left: -67px;
  }

  .timer-icon.stopped {
    border-left: 17px solid #9E9E9E;
    border-right: 20px solid transparent;
  }
  
  .reset-icon {
    margin-top: 80px; 
    top: 0;
    margin-left: 67px;
    width: 20px;
    height: 20px;
    background-color: #9E9E9E;
    position: absolute; 
    bottom: 40px;
    border: none;
  }

  .reset-icon-white {
    background-color: #FFFFFF;
  }
</style>
<script>
export default {
  name: 'StopwatchTimer',
  data() {
    return {
      running: false,
      elapsedTime: 0,
      lastTime: 0
    };
  },
  computed: {
    formattedTime() {
      // let hours = Math.floor(this.elapsedTime / 3600000);
      // let minutes = Math.floor((this.elapsedTime % 3600000) / 60000);
      // let seconds = Math.floor((this.elapsedTime % 60000) / 1000);
      let hours = Math.floor(this.elapsedTime / 3600000);
      let minutes = Math.floor((this.elapsedTime % 3600000) / 60000);
      let seconds = Math.floor((this.elapsedTime % 60000) / 1000);

      if (hours > 0) {
        return `${hours}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
      } else if (minutes > 0) {
        return `${minutes}:${seconds.toString().padStart(2, '0')}`;
      } else {
        return `${seconds}`;
      }
    },
    timerIcon() {
      return this.running ? 'timer-icon running' : 'timer-icon stopped';
    }
  },
  methods: {
    toggle() {
      if (this.running) {
        this.stop();
      } else {
        this.start();
      }
    },
    start() {
      if (!this.running) {
        this.running = true;
        this.lastTime = Date.now();
        this.tick();
      }
    },
    stop() {
      this.running = false;
    },
    reset() {
      this.elapsedTime = 0;
      this.stop();
    },
    tick() {
      const now = Date.now();
      const  timeDiff = now - this.lastTime;
      this.elapsedTime += timeDiff;
      this.lastTime = now;

      if (this.running) {
        requestAnimationFrame(this.tick);
      }
    }
  }
};
</script>