<template>
  <div>
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <p>
        List of answers
      </p>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object
  }
}
</script>