<template>
  <div class="container">
    <header>
      <h1>Quizzes</h1>
      <input
        @change="handleChange"
        v-model="search"
        type="text"
        placeholder="Search..."
      />
    </header>
    <div class="options-container">
      <!-- <div :key="quiz.id" class="card">
          <img :src="quiz.img" :alt="quiz.name" />
          <div class="card-text">
            <h2>{{ quiz.name }}</h2>
            <p>{{ quiz.questions.length }} questions</p>
          </div>
        </div> -->
      <TheCard v-for="quiz in quizes" :key="quiz.id" :quiz="quiz" />
    </div>
  </div>
</template>

<script setup>
import q from "../data/quizes.json";
import { ref, watch } from "vue";
import TheCard from "@/components/TheCard";

const quizes = ref(q);
const search = ref("");

watch(search, () => {
  quizes.value = q.filter((item) =>
    item.name.toLowerCase().includes(search.value)
  );
});
</script>

<style scoped>
.container {
  max-width: 1000px;
  margin: 0 auto;
}
header {
  margin-bottom: 10px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

header h1 {
  margin-right: 30px;
  font-weight: bold;
}

header input {
  border: none;
  background-color: rgba(128, 128, 128, 0.1);
  border-radius: 1px;
  padding: 10px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 50px;
}
</style>
