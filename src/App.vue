<template>
  <Header
    :numCorrect="numCorrect"
    :size="questions.length"
    :numTotal="numTotal"
  />
  <div class="container p-5 bg-light rounded-3">
    <div v-if="doneSelection === false">
      <h2 class="text-start">Select Difficulty:</h2>
      <select
        class="form-select form-select-lg mb-3"
        aria-label=".form-select-lg example"
        v-model="difficulty"
      >
        <option selected value="">Any Difficulty</option>
        <option value="easy">Easy</option>
        <option value="medium">Medium</option>
        <option value="hard">Hard</option>
      </select>
      <h2 class="text-start">Select Category:</h2>
      <select
        class="form-select form-select-lg mb-3"
        aria-label=".form-select-lg example"
        v-model="category"
      >
        <option selected value="">Any Category</option>
        <option value="21">Sports</option>
        <option value="27">Animals</option>
        <option value="26">Celebrities</option>
        <option value="9">General Knowledge</option>
        <option value="11">Entertainment: Film</option>
      </select>
      <h2 class="text-start">Number of Questions:</h2>
      <select
        class="form-select form-select-lg mb-3"
        aria-label=".form-select-lg example"
        v-model="questionAmount"
      >
        <option selected value="10">10</option>
        <option value="20">20</option>
        <option value="30">30</option>
        <option value="40">40</option>
      </select>
      <button class="btn btn-primary" type="button" @click="start">
        Start
      </button>
    </div>
    <div
      v-if="!questions.length && doneSelection"
      class="spinner-border"
      role="status"
    >
      <span class="visually-hidden">Loading...</span>
    </div>
    <div v-if="questions.length && doneSelection">
      <QuestionBox
        :currentQuestion="questions[index]"
        :done="done"
        :next="next"
        :increment="increment"
      />
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import axios from "axios";
export default {
  name: "App",
  components: { Header, QuestionBox },
  data() {
    return {
      params: "",
      questionAmount: 10,
      category: "",
      difficulty: "",
      doneSelection: false,
      done: false,
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    start() {
      this.params = `https://opentdb.com/api.php?amount=${this.questionAmount}`;
      if (this.category != "") {
        this.params += `&category=${this.category}`;
      }
      if (this.difficulty != "") {
        this.params += `&difficulty=${this.difficulty}`;
      }
      this.params += "&encode=base64";
      this.doneSelection = true;
      this.getApi();
    },
    next() {
      this.index++;
      if (this.index + 1 === this.questions.length) {
        this.done = true;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
    getApi() {
      axios
        .get(this.params)
        .then((response) => (this.questions = response.data.results));
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0;
  padding: 10px;
  background-color: #212529;
  min-height: 100vh;
}
</style>
