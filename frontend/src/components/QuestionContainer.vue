<script setup>
import { ref, onMounted } from "vue";
import Question from "./Question.vue";
import Timer from "./Timer.vue";
import Results from "./Results.vue";

const questions = ref([]);

const currentQuestionIndex = ref(0);
const score = ref(0);
const attendedQuestions = ref();
const quizCompleted = ref(false);
const isLoading = ref(true); // Ref to track loading state
const errorMessage = ref(null); // Ref to handle any errors

const handleAnswer = (isCorrect) => {
	attendedQuestions.value[currentQuestionIndex.value] = true;
	score.value += isCorrect ? 1 : -1 / 3;
	if (currentQuestionIndex.value < questions.value.length - 1) {
		currentQuestionIndex.value++;
	}
};

const skipQuestion = () => {
	if (currentQuestionIndex.value < questions.value.length - 1) {
		currentQuestionIndex.value++;
	}
};

const jumpToQuestion = (index) => {
	currentQuestionIndex.value = index;
};

const submitQuiz = () => {
	quizCompleted.value = true;
};

const handleTimeUp = () => {
	submitQuiz();
};

const restartQuiz = () => {
	currentQuestionIndex.value = 0;
	score.value = 0;
	attendedQuestions.value = new Array(questions.value.length).fill(false);
	quizCompleted.value = false;
};

const fetchQuestions = async () => {
	try {
		const response = await fetch("http://localhost:8088/api/questions");
		if (!response.ok) {
			throw new Error(`HTTP error! Status: ${response.status}`);
		}
		const data = await response.json();
		questions.value = data;
    console.log(data);
    attendedQuestions.value = new Array(questions.value.length).fill(false);
    
	} catch (error) {
		errorMessage.value = error.message;
	} finally {
		isLoading.value = false;
	}
};

onMounted(() => {
	fetchQuestions();
});

</script>

<template>
	<div class="max-w-2xl mx-auto">
		<div v-if="!quizCompleted" class="mb-6 flex justify-between items-center">
			<Timer :timeLimit="120" @time-up="handleTimeUp" />
		</div>
		<div v-if="!quizCompleted && currentQuestionIndex < questions.length">
			<Question
				:question="questions[currentQuestionIndex]"
				:questionNumber="currentQuestionIndex + 1"
				:totalQuestions="questions.length"
				:isAttended="attendedQuestions[currentQuestionIndex]"
				@answer-selected="handleAnswer"
				@skip-question="skipQuestion"
			/>
		</div>
		<div v-else-if="!quizCompleted" class="text-center">
			<h2 class="text-2xl font-bold mb-4 text-blue-700">Review Your Answers</h2>
			<p class="mb-4 text-gray-600">
				You've reached the end of the quiz. You can review your answers or
				submit your quiz.
			</p>
			<button
				@click="submitQuiz"
				class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
			>
				Submit Quiz
			</button>
		</div>
		<div v-else>
			<Results
				:score="score"
				:totalQuestions="questions.length"
				:attendedQuestions="attendedQuestions"
			/>
			<button
				@click="restartQuiz"
				class="mt-6 bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
			>
				Restart Test
			</button>
		</div>
		<div v-if="!quizCompleted" class="mt-8">
			<h3 class="text-lg font-semibold mb-3 text-gray-700">Question Status:</h3>
			<div class="flex flex-wrap gap-2">
				<button
					v-for="(attended, index) in attendedQuestions"
					:key="index"
					@click="jumpToQuestion(index)"
					:class="[
						'w-10 h-10 rounded-full font-bold text-sm transition duration-300 ease-in-out transform hover:scale-110',
						attended ? 'bg-green-500 text-white' : 'bg-red-500 text-white',
						index === currentQuestionIndex ? 'ring-4 ring-blue-300' : '',
					]"
				>
					{{ index + 1 }}
				</button>
			</div>
		</div>
	</div>
</template>
