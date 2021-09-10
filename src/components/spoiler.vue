<script setup lang="ts">
import { ref } from "vue";
const show = ref(false);
</script>

<template>
  <div class="text-center my-2 text-2xl">
    <div class="border-green-600 border-2 bg-green-100 p-4 font-bold text-green-900" v-if="show">
      <slot></slot>
    </div>
    <button
      class="
        flex-shrink-1
        text-base
        font-semibold
        py-2
        px-4
        rounded-lg
        shadow-md
        focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 focus:ring-offset-purple-200
        my-2
        ml-1
        mr-1
        bg-purple-600
        text-white
        hover:bg-purple-700'
      "
      v-else
      v-on:click="show = true"
    >
      Reveal
    </button>
  </div>
</template>

<style scoped></style>
