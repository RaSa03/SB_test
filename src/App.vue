<template>
  <div class="wrapper">
    <div class="body">
      <div v-for="(timer, index) in timers" :key="index" class="rectangle">
        <div class="timer-piece">
          <span>{{ formatTime(timer.time) }}</span>
        </div>
        <div class="buttons-piece">
          <button
            v-if="!timer.isRunning"
            @click="startTimer(index)"
            :disabled="timer.isRunning"
          >
            <img src="./assets/icons/play.png" alt="" />
          </button>
          <button
            v-else
            @click="pauseTimer(index)"
            :disabled="!timer.isRunning"
          >
            <img src="./assets/icons/pause.png" alt="" />
          </button>

          <button @click="resetTimer(index)">
            <img src="./assets/icons/stop.png" alt="" />
          </button>
        </div>
      </div>
      <div class="rectangle plus">
        <button @click="addTimer">
          <img src="./assets/icons/plus.png" alt="" />
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";

const timers = reactive([]);
const intervalIds = ref([]);

const addTimer = () => {
  timers.push({
    time: 0,
    isRunning: false,
  });
  intervalIds.value.push(null);
};

const startTimer = (index) => {
  const intervalId = setInterval(() => {
    timers[index].time++;
  }, 1000);
  intervalIds.value[index] = intervalId;
  timers[index].isRunning = true;
};

const pauseTimer = (index) => {
  clearInterval(intervalIds.value[index]);
  timers[index].isRunning = false;
};

const resetTimer = (index) => {
  clearInterval(intervalIds.value[index]);
  timers[index].time = 0;
  timers[index].isRunning = false;
};

const formatTime = (value) => {
  const hours = Math.floor(value / 3600);
  const minutes = Math.floor((value - hours * 3600) / 60);
  const seconds = value % 60;
  if (hours > 0) {
    return `${hours < 10 ? "0" : ""}${hours}:${
      minutes < 10 ? "0" : ""
    }${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
  } else if (minutes > 0) {
    return `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
  } else {
    return `${seconds}`;
  }
};
</script>

<style scoped></style>
