<script setup lang="ts">
import { defineProps } from "vue";

const props = defineProps<{ value: 0 | 1 }>();
</script>

<template>
  <span class="bg-black border w-4 h-4 inline-block" v-bind:class="props.value === 0 ? 'bg-white' : 'bg-black'" />
</template>

<style scoped></style>
