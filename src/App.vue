<template>
  <div id="app">
    <Header :score="score" :questionsAnswered="index" />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset-sm="3">
          <QuestionsList
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :incrementScore="incrementScore"
            :incrementQuestionsAnswered="incrementQuestionsAnswered"
            :showNextButton="amountOfQuestions === index + 1 ? false : true"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header';
import QuestionsList from './components/QuestionsList';

export default {
  name: 'App',
  components: {
    Header,
    QuestionsList,
  },
  data() {
    return {
      questions: [],
      index: 0,
      score: 0,
      hasSubmitted: false,
      amountOfQuestions: 3,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    incrementScore() {
      this.score++;
    },
    incrementQuestionsAnswered() {
      this.questionsAnswered++;
    },
  },
  mounted() {
    fetch(`https://opentdb.com/api.php?amount=${this.amountOfQuestions}&type=multiple`, {
      method: 'GET',
    })
      .then(res => res.json())
      .then(data => {
        this.questions = data.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
