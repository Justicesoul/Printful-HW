<template>
  <div class="results">
    <h2>Thank You, {{ username }}!</h2>
    <span>
      You've got
      <span class="results__text--green results__text">{{
        quizCorrectAnswersCount
      }}</span>
      correct {{ quizCorrectAnswersCount === 1 ? 'answer' : 'answers' }}, out of
      <span class="results__text--blue results__text">{{
        quizQuestions.length
      }}</span>
      questions!
    </span>
    <button class="button margin-top" @click="playAgain">Play again</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Results',
  props: {
    playAgain: Function,
    username: String,
    quizCorrectAnswersCount: Number,
    quizQuestions: Array,
  },
});
</script>
<style lang="scss">
.results {
  width: 500px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  border-radius: var(--br-medium);
}

.results__text--green {
  color: rgb(31, 190, 31);
}

.results__text--blue {
  color: var(--clr-blue);
}

.results__text {
  text-decoration: underline;
  font-weight: 700;
}

@media (max-width: 520px) {
  .results {
    width: 280px;
  }
}
</style>
