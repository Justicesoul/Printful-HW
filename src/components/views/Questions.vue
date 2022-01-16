<template>
  <Loading v-if="dataLoading" />
  <div v-else class="questions">
    <h2>Question {{ `${quizQuestionNumber + 1}/${quizQuestions.length}` }}</h2>
    <ProgressBar
      :quizQuestionNumber="quizQuestionNumber"
      :quizQuestions="quizQuestions"
    />
    <div>
      <h2>{{ quizQuestion.title }}</h2>
    </div>
    <div class="questions__answer-buttons">
      <button
        v-for="{ title, id } in quizAnswers"
        :key="id"
        class="questions__button"
        @click="activeAnswer(id)"
        :class="{ active: this.answerId === id }"
      >
        {{ title }}
      </button>
    </div>

    <button
      :disabled="disabledAnswerButton"
      @click="nextQuestionHandler"
      class="questions__button margin-top"
    >
      {{
        quizQuestionNumber + 1 === quizQuestions.length
          ? 'Get the results'
          : 'Next question'
      }}
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Loading from '../small-components/Loading.vue';
import ProgressBar from '../small-components/ProgressBar.vue';

export default defineComponent({
  name: 'Questions',
  components: {
    Loading,
    ProgressBar,
  },
  props: {
    quizQuestionNumber: Number,
    quizQuestion: Object,
    quizAnswers: Array,
    quizQuestions: Array,
    answerId: Number,
    disabledAnswerButton: Boolean,
    dataLoading: Boolean,
    activeAnswer: Function,
    nextQuestionHandler: Function,
  },
});
</script>
<style lang="scss">
.questions {
  width: 700px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  border-radius: var(--br-medium);
}

.questions__answer-buttons {
  display: grid;
  grid-template-columns: repeat(2, 300px);
  gap: 20px;
  justify-items: center;
}

.questions__button {
  width: 300px;
  font-size: 18px;
  padding: 10px;
  background-color: var(--clr-blue);
  color: var(--clr-white);
  border-radius: var(--br-small);
  border: 1px solid transparent;
  transition: 0.3s;
  cursor: pointer;
  &:not(:disabled):hover,
  &.active {
    color: var(--clr-blue);
    border: 1px solid var(--clr-blue);
    background-color: var(--clr-white);
  }
  &:disabled {
    opacity: 0.3;
    cursor: not-allowed;
  }
}

@media (max-width: 720px) {
  .questions {
    width: 280px;
  }

  .questions__button {
    width: 250px;
  }

  .questions__answer-buttons {
    grid-template-columns: 200px;
    gap: 5px;
  }
}
</style>
