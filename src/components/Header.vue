<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Vue tutorial</b-nav-item>
      <b-nav-item disabled>Counter: {{ score }}/{{ questionsAnswered }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    score: Number,
    questionsAnswered: Number,
  },
};
</script>
