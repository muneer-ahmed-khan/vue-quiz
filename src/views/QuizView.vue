<script setup>
import TheQuestion from "@/components/TheQuestion.vue";
import QuizHeader from "@/components/QuizHeader.vue";
import TheResult from "@/components/TheResult.vue";
import { useRoute } from "vue-router";
import { ref, computed } from "vue";
import q from "@/data/quizes.json";

const route = useRoute();

const quizId = parseInt(route.params.id);
const quiz = q.find((q) => q.id === quizId);
const currentQuestionIndex = ref(0);
const numberOfCorrectAnswer = ref(0);
const showResult = ref(false);

// const questionStatus = ref(
//   `${currentQuestionIndex.value}/${quiz.questions.length}`
// );

// watch(currentQuestionIndex, () => {
//   questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`;
// });

const questionStatus = computed(
  () => `${currentQuestionIndex.value}/${quiz.questions.length}`
);

const barPercentage = computed(
  () => `${(currentQuestionIndex.value / quiz.questions.length) * 100}`
);

const onOptionSelected = (isCorrect) => {
  if (isCorrect) {
    numberOfCorrectAnswer.value++;
  }

  if (quiz.questions.length - 1 === currentQuestionIndex.value) {
    showResult.value = true;
  }

  currentQuestionIndex.value++;
};
</script>

<template>
  <div>
    <QuizHeader
      :questionStatus="questionStatus"
      :barPercentage="barPercentage"
    />
    <div>
      <TheQuestion
        v-if="!showResult"
        @selectOption="onOptionSelected"
        :question="quiz.questions[currentQuestionIndex]"
      />

      <TheResult
        v-else
        :quizQuestionLength="quiz.questions.length"
        :numberOfCorrectAnswers="numberOfCorrectAnswer"
      />
    </div>
  </div>
</template>
