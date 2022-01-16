<template>
  <div class="progress-bar">
    <div
      class="progress-bar__completed"
      :style="{
        width:
          (this.quizQuestionNumber / this.quizQuestions.length) * 100 + '%',
      }"
    ></div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ProgressBar',
  props: {
    quizQuestions: Array,
    quizQuestionNumber: Number,
  },
});
</script>
<style lang="scss">
.progress-bar {
  width: 100%;
  height: 15px;
  border-radius: var(--br-big);
  border: none;
  background-color: rgb(173, 173, 240);
}

.progress-bar__completed {
  height: 100%;
  border-radius: var(--br-big);
  border: none;
  background-color: var(--clr-blue);
}
</style>
