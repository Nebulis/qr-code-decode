<script setup lang="ts">
import { defineProps } from "vue";

type Case = 0 | 1;
const props = withDefaults(defineProps<{ cases: Case[][]; addClass?: (row: number, col: number, value: Case) => string }>(), { addClass: () => "" });
</script>

<template>
  <div v-for="(row, indexRow) in cases" class="p-0 m-0 h-6 case-container">
    <div v-for="(col, indexCol) in row" class="w-6 h-6 inline-block mb-0 relative">
      <span class="w-6 h-6 inline-block mb-0 case" v-bind:class="[col === 1 ? 'bg-black' : 'bg-white']" />
      <span class="w-6 h-6 inline-block mb-0 absolute top-0 left-0 case2" v-bind:class="[addClass(indexRow, indexCol, col)]" />
    </div>
  </div>
</template>

<style scoped></style>
