<template>
  <div class="error">
    <h2 class="error__heading">
      Some data loading error. Please try again later.
    </h2>
    <button class="button margin-top" @click="refresh">Try again</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'DataError',
  methods: {
    refresh() {
      location.reload();
    },
  },
});
</script>

<style lang="scss">
.error {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.error__heading {
  color: red;
}
</style>
