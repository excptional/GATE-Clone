<script setup>
import { ref } from 'vue';

const props = defineProps({
  question: Object,
  questionNumber: Number,
  totalQuestions: Number,
  isAttended: Boolean,
});

const emit = defineEmits(['answer-selected', 'skip-question']);

const selectedAnswer = ref(null);

const selectAnswer = (option) => {
  selectedAnswer.value = option;
};

const submitAnswer = () => {
  if (selectedAnswer.value) {
    emit('answer-selected', selectedAnswer.value === props.question.correctAnswer);
    selectedAnswer.value = null;
  }
};
</script>


<template>
    <div class="bg-white rounded-lg border-2 p-6">
      <h2 class="text-2xl font-semibold mb-4 text-blue-600">Question {{ questionNumber }} of {{ totalQuestions }}</h2>
      <p class="mb-6 text-gray-700 text-lg">{{ question.text }}</p>
      <div class="space-y-3">
        <button
          v-for="option in question.options"
          :key="option"
          @click="selectAnswer(option)"
          class="w-full text-left p-3 rounded-lg transition duration-300 ease-in-out transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-blue-500"
          :class="[
            selectedAnswer === option ? 'bg-indigo-100 text-blue-700 font-medium' : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
          ]"
        >
          {{ option }}
        </button>
      </div>
      <div class="mt-8 flex justify-between">
        <button
          @click="$emit('skip-question')"
          class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
        >
          Skip
        </button>
        <button
          @click="submitAnswer"
          :disabled="!selectedAnswer"
          class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          Submit
        </button>
      </div>
    </div>
  </template>
