<template>
  <div class="container">
    <MainHeading :activeView="activeView" />
    <Form
      v-if="activeView === 'Form'"
      :quizCategories="quizCategories"
      :submitHandler="submitHandler"
      :username="username"
      :select-value="category"
      :formInitialState="formInitialState"
      @setUsername="username = $event"
      @setSelectValue="category = $event"
    />
    <Questions
      v-if="activeView === 'Questions'"
      :quizQuestionNumber="quizQuestionNumber"
      :quizQuestion="quizQuestion"
      :quizAnswers="quizAnswers"
      :quizQuestions="quizQuestions"
      :answerId="answerId"
      :disabledAnswerButton="disabledAnswerButton"
      :dataLoading="dataLoading"
      :activeAnswer="activeAnswer"
      :nextQuestionHandler="nextQuestionHandler"
    />
    <Results
      v-if="activeView === 'Results'"
      :username="username"
      :quizCorrectAnswersCount="quizCorrectAnswersCount"
      :quizQuestions="quizQuestions"
      :playAgain="playAgain"
    />
    <DataError v-if="activeView === 'Error'" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import Form from './components/views/Form.vue';
import Results from './components/views/Results.vue';
import MainHeading from './components/small-components/MainHeading.vue';
import Questions from './components/views/Questions.vue';
import DataError from './components/views/DataError.vue';

axios.defaults.baseURL = 'https://printful.com/';

type Quiz = {
  id: number;
  title: string;
};

export default defineComponent({
  name: 'Home',
  components: {
    Form,
    Results,
    MainHeading,
    Questions,
    DataError,
  },
  data: () => ({
    quizCategories: [] as Quiz[],
    quizQuestions: [] as Quiz[],
    quizAnswers: [] as Quiz[],
    userAnswers: [] as number[],
    quizQuestion: {} as Quiz,
    quizQuestionNumber: 0,
    answerId: 0,
    category: '',
    username: '',
    answersUrl: '',
    quizCorrectAnswersCount: 0,
    activeView: 'Form',
    formInitialState: true,
    disabledAnswerButton: true,
    dataLoading: true,
  }),
  mounted() {
    axios
      .get('test-quiz.php?action=quizzes')
      .then(({ data }) => {
        this.quizCategories = data;
      })
      .catch((error) => {
        if (error) {
          this.activeView = 'Error';
        }
      });
  },
  methods: {
    async getQuestions() {
      await axios
        .get(`test-quiz.php?action=questions&quizId=${this.category}`)
        .then(({ data }) => {
          this.quizQuestions = data;
        })
        .catch((error) => {
          if (error) {
            this.activeView = 'Error';
          }
        });
    },
    getAnswers() {
      axios
        .get(
          `test-quiz.php?action=answers&quizId=${this.category}&questionId=${this.quizQuestion.id}`
        )
        .then(({ data }) => {
          this.quizAnswers = data;
        })
        .catch((error) => {
          if (error) {
            this.activeView = 'Error';
          }
        })
        .finally(() => {
          this.dataLoading = false;
        });
    },
    getCorrectAnswers() {
      axios
        .get(
          `test-quiz.php?action=submit&quizId=${this.category}${this.answersUrl}`
        )
        .then(({ data }) => {
          this.quizCorrectAnswersCount = data.correct;
        })
        .catch((error) => {
          if (error) {
            this.activeView = 'Error';
          }
        });
    },
    async submitHandler() {
      this.formInitialState = false;
      if (this.category && this.username) {
        this.activeView = 'Questions';
        await this.getQuestions();
        this.quizQuestion = this.quizQuestions[0];
        this.getAnswers();
      }
    },
    submitAnswer() {
      this.userAnswers.push(this.answerId);
      this.answersUrl += `&answers[]=${this.answerId}`;
      this.answerId = 0;
      this.disabledAnswerButton = true;
      this.dataLoading = true;
    },
    nextQuestionHandler() {
      if (this.quizQuestionNumber === this.quizQuestions.length - 1) {
        this.submitAnswer();
        this.activeView = 'Results';
        this.getCorrectAnswers();
      } else {
        this.quizQuestionNumber += 1;
        this.quizQuestion = this.quizQuestions[this.quizQuestionNumber];
        this.getAnswers();
        this.submitAnswer();
      }
    },
    activeAnswer(answerId: number) {
      this.answerId = answerId;
      this.disabledAnswerButton = false;
    },
    playAgain() {
      this.userAnswers = [];
      this.quizQuestionNumber = 0;
      this.username = '';
      this.category = '';
      this.answersUrl = '';
      this.activeView = 'Form';
      this.formInitialState = true;
    },
  },
});
</script>

<style lang="scss">
@import 'assets/styles/global';

.container {
  position: fixed;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
}

@media (max-width: 720px) {
  .container {
    top: 50%;
  }
}
</style>
