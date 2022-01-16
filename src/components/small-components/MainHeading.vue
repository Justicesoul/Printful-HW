<template>
  <h1>{{ `Printful Quiz: ${activeView}` }}</h1>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MainHeading',
  props: {
    activeView: String,
  },
});
</script>
