<template>
  <img
    class="loading__image"
    src="../../assets/images/loading.gif"
    alt="loading"
  />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Loading',
});
</script>

<style lang="scss">
.loading__image {
  width: 300px;
}
</style>
