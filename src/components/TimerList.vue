<template>
  <div>
    <div
      v-for="(timer) in timers"
      :key="timer.id"
      class="timer-list-container"
      :style="{ left: (timer.leftOffset) + 'px', top: (timer.topOffset) + 'px' }"
    >
      <stopwatch-timer />
    </div>
    <div
      class="timer-list-container"
      :style="{ left: (addTimerLeftOffset) + 'px', top: (addTimerTopOffset) + 'px' }"
    >
      <button @click="addTimer" class="AddTimerIcon"></button>
    </div>
  </div>
</template>
  
  <style>
    .timer-list-container {
      width: 225px;
      height: 120px;
      background-color: #696969;
      display: flex;
      justify-content: center;
      align-items: flex-end;
      position: absolute;
      top: 72px;
    }
  
    .AddTimerIcon {
      width: 100%;
      height: 100%;
      background-color: #696969;
      border: none;
      overflow: hidden;
      cursor: pointer;
    }
  
    .AddTimerIcon:before,
    .AddTimerIcon:after {
      content: "";
      position: absolute;
      width: 20px;
      height: 2px;
      background-color: #9e9e9e;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  
    .AddTimerIcon:before {
      transform: translate(-50%, -50%) rotate(90deg);
    }
  </style>
  
  <script>
  import StopwatchTimer from "./Timer.vue";
  
  export default {
    name: "TimerList",
    components: {
      StopwatchTimer,
    },
    data() {
        const timersPerLine = window.innerWidth >= 1024 ? 3 : window.innerWidth >= 768 ? 2 : 1;
      return {
        timers: [],
        nextId: 1,
        addTimerLeftOffset: window.innerWidth >= 1024 ? 212 : window.innerWidth >= 768 ? 135 : 46,
        addTimerTopOffset: 72,
        timersPerLine,
      };
    },
    methods: {
      addTimer() {
        const newTimer = {
          id: this.nextId++,
          leftOffset: this.timers.length === 0 ? this.addTimerLeftOffset : this.addTimerLeftOffset,
          topOffset: this.addTimerTopOffset,
        };
        this.timers.push(newTimer);
        this.addTimerLeftOffset = (this.timers.length % this.timersPerLine === 0 ? (window.innerWidth >= 1024 ? 212 : window.innerWidth >= 768 ? 135 : 46) : this.addTimerLeftOffset + 275);
        this.addTimerTopOffset += (this.timers.length % this.timersPerLine === 0 ? 165 : 0);
      },
    },
  };
  </script>