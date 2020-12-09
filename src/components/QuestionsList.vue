<template>
  <div>
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <ul>
        <li 
          v-for="(answer, index) in answers"
          :key="index"
          >
          {{ answer }}
        </li>
      </ul>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  export default {
    props: {
      currentQuestion: {
        type: Object,
      },
      next: {
        type: Function
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers];
        answers.push(this.currentQuestion.correct_answer);
        return answers;
      }
    }
  }
</script>