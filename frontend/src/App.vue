<script setup>
import QuestionContainer from './components/QuestionContainer.vue';
</script>


<template>
    <div class="min-h-screen sm:bg-blue-200 bg-transparent py-6 flex flex-col justify-center sm:py-12 font-poppins">
      <div class="relative py-3 sm:max-w-xl sm:mx-auto">
        <div class="relative px-4 py-10 bg-white shadow-lg sm:rounded-3xl sm:p-10">
          <h1 class="text-4xl font-bold mb-8 text-center text-gray-800 tracking-wide">GATE Mock Test</h1>
          <QuestionContainer />
        </div>
      </div>
    </div>
  </template>
  