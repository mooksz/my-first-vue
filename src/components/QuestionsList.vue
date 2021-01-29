<template>
  <div>
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group :class="hasSubmitted ? 'submitted' : null">
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="selectedAnswerIndex === null || hasSubmitted"
        >Submit</b-button
      >
      <b-button v-if="showNextButton" @click="next" variant="success" :disabled="!hasSubmitted">Next</b-button>
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
    showNextButton: Boolean,
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
    answerClass(index) {
      const baseClass = 'list-group-item--';

      //Has not submitted yet and is select
      if (!this.hasSubmitted && this.selectedAnswerIndex === index) {
        return `${baseClass}selected`;
      }

      //Has submitted, this is the correct answer
      if (this.hasSubmitted && this.correctAnswerIndex === index) {
        return `${baseClass}correct`;
      }

      //Has submitted and this one was selected, but it was wrong
      if (this.hasSubmitted && this.correctAnswerIndex !== index && this.selectedAnswerIndex === index) {
        return `${baseClass}incorrect`;
      }

      return null;
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

.submitted .list-group-item:hover {
  background-color: #fff;
  cursor: default;
}

.list-group-item.list-group-item--selected,
.list-group-item.list-group-item--selected:hover {
  background-color: #007bff;
  color: #fff;
}

.list-group-item.list-group-item--correct,
.list-group-item.list-group-item--correct:hover {
  background-color: #28a745;
  color: #fff;
}

.list-group-item.list-group-item--incorrect,
.list-group-item.list-group-item--incorrect:hover {
  background-color: #dc3545;
  color: #fff;
}

.btn {
  margin: 0 0.5rem;
}
</style>
