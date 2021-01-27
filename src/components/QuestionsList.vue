<template>
  <div>
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedAnswerIndex === index ? 'list-group-item--selected' : null]"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="selectedAnswerIndex === null || hasSubmitted"
        >Submit</b-button
      >
      <b-button @click="next" variant="success" :disabled="!hasSubmitted">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    incrementScore: Function,
    incrementQuestionsAnswered: Function,
  },
  data() {
    return {
      selectedAnswerIndex: null,
      correctAnswerIndex: null,
      shuffledAnswers: [],
      hasSubmitted: false,
    };
  },
  methods: {
    //Methods we can call
    selectAnswer(index) {
      this.selectedAnswerIndex = index;
    },
    shuffleAnswers() {
      this.shuffledAnswers = _.shuffle([
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ]);

      this.correctAnswerIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      this.hasSubmitted = true;
      let isCorrect = false;

      if (this.selectedAnswerIndex === this.correctAnswerIndex) {
        isCorrect = true;
      }

      if (isCorrect) {
        this.incrementScore();
      }

      this.incrementQuestionsAnswered();
    },
  },
  computed: {
    //Data we can compute and use in the component (not initiated by me)
    answers() {
      return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
    },
  },
  watch: {
    //This watched the property you name it for changes
    // Way 1
    // currentQuestion() {
    //   this.selectedAnswerIndex = null;
    //   this.shuffleAnswers();
    // },

    //Way 2 (run immediatly)
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedAnswerIndex = null;
        this.hasSubmitted = false;
        this.shuffleAnswers();
      },
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 1rem;
}

.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.list-group-item.list-group-item--selected {
  background-color: #007bff;
  color: #fff;
}

.list-group-item.list-group-item--correct {
  background-color: #28a745;
  color: #fff;
}

.list-group-item.list-group-item--incorrect {
  background-color: #dc3545;
  color: #fff;
}

.btn {
  margin: 0 0.5rem;
}
</style>
