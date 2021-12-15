<template>
  <div class="container">
    <div class="container py-5">
      <div class="flex-column d-flex">
        <div class="container">
          <p class="fs-2 fw-bold">
            {{ decodedQuestion(currentQuestion.question) }}
          </p>
        </div>
        <div class="container">
          <ul class="list-group">
            <li
              class="list-group-item p-3"
              v-for="(answer, index) in shuffledAnswers"
              :key="index"
              @click="selectedAnswer(index)"
              :class="answerClass(index)"
            >
              {{ decodedAnswer(answer) }}
            </li>
          </ul>
        </div>
      </div>
      <button
        class="btn btn-primary"
        type="button"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </button>
      <button
        @click="next"
        :disabled="done || !answered"
        class="btn btn-success"
        type="button"
      >
        Next
      </button>
    </div>
  </div>
</template>
<script>
import _ from "lodash";
export default {
  props: {
    done: Boolean,
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      answered: false,
      correctIndex: null,
      selectedIndex: null,
      shuffledAnswers: [],
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion() {
      this.answered = false;
      this.selectedIndex = null;
      this.shuffleAnswers();
    },
  },
  methods: {
    decodedQuestion(question) {
      return window.atob(question);
    },
    decodedAnswer(answer) {
      return window.atob(answer);
    },
    selectedAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;

      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },
  mounted() {
    this.shuffleAnswers();
  },
};
</script>
<style scoped>
.list-group {
  margin-bottom: 50px;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: lightblue;
}
.selected:hover {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.correct:hover {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
.incorrect:hover {
  background-color: red;
}
</style>