<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const props = defineProps({
  timeLimit: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['time-up']);

const timeRemaining = ref(props.timeLimit);
let timer;

const formatTime = (seconds) => {
  const minutes = Math.floor(seconds / 60);
  const remainingSeconds = seconds % 60;
  return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`;
};

const startTimer = () => {
  timer = setInterval(() => {
    if (timeRemaining.value > 0) {
      timeRemaining.value--;
    } else {
      clearInterval(timer);
      emit('time-up');
    }
  }, 1000);
};

onMounted(() => {
  startTimer();
});

onUnmounted(() => {
  clearInterval(timer);
});
</script>

<template>
    <div class="text-md ml-auto font-bold text-blue-600">
      Time Remaining: {{ formatTime(timeRemaining) }}
    </div>
  </template>
