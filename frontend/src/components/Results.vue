<script setup>
import { computed } from 'vue';

const props = defineProps({
  score: Number,
  totalQuestions: Number,
  attendedQuestions: Array
});

const attendedCount = computed(() => props.attendedQuestions.filter(Boolean).length);
const unattemptedCount = computed(() => props.totalQuestions - attendedCount.value);
const correctAnswers = computed(() => Math.round(props.score));
const incorrectAnswers = computed(() => attendedCount.value - correctAnswers.value);
</script>

<template>
    <div class="bg-white rounded-lg border-2 p-6">
      <h2 class="text-3xl font-bold mb-6 text-blue-700">Results</h2>
      <div class="mb-6">
        <p class="text-2xl mb-2">Your Score: <span class="font-bold text-blue-600">{{ score.toFixed(2) }}</span></p>
        <p class="text-xl">Out of {{ totalQuestions }} questions</p>
      </div>
      <div class="mb-6">
        <h3 class="text-xl font-semibold mb-2 text-gray-700">Questions Attempted:</h3>
        <p class="text-lg">{{ attendedCount }} out of {{ totalQuestions }}</p>
      </div>
      <div class="m-4">
        <h3 class="text-xl font-semibold mb-2 text-gray-700">Performance:</h3>
        <p class="text-lg">Correct Answers: {{ correctAnswers }}</p>
        <p class="text-lg">Incorrect Answers: {{ incorrectAnswers }}</p>
        <p class="text-lg">Unattempted: {{ unattemptedCount }}</p>
      </div>
    </div>
  </template>
