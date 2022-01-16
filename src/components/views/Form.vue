<template>
  <form class="form" @submit.prevent="submitHandler">
    <label class="form__label">
      Enter your name
      <input
        class="form__input"
        type="text"
        name="name"
        placeholder="Enter your name"
        @input="$emit('setUsername', $event.target.value)"
      />
      <span class="form__error-message" v-if="!username && !formInitialState"
        >Please, enter your name</span
      >
    </label>

    <label class="form__label"
      >Choose quiz category
      <select
        class="form__input"
        @change="$emit('setSelectValue', $event.target.value)"
      >
        <option value="">- Choose category -</option>
        <option v-for="{ title, id } in quizCategories" :key="id" :value="id">
          {{ title }}
        </option>
      </select>
      <span class="form__error-message" v-if="!selectValue && !formInitialState"
        >Please, choose quiz category</span
      >
    </label>
    <button class="button">Start</button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Form',
  props: {
    quizCategories: Array,
    submitHandler: Function,
    username: String,
    selectValue: String,
    formInitialState: Boolean,
  },
});
</script>
<style lang="scss">
.form {
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  border-radius: var(--br-medium);
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.form__label {
  margin-bottom: 20px;
  font-size: 18px;
}

.form__input {
  display: block;
  margin-top: 5px;
  width: 300px;
  padding: 5px 7px;
  border: 1px solid black;
  border-radius: var(--br-small);
}

.form__error-message {
  color: red;
  font-size: 12px;
}

@media (max-width: 720px) {
  .form__input {
    width: 250px;
  }
}
</style>
