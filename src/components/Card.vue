<template>
  <div
    :class="{ 'selected': isInFocus, 'card':true }"
    :style="`height: ${height}px; line-height:${height}px;`"
  >
    {{cardText}}
  </div>
</template>

<script setup>
import { defineProps } from "vue";

defineProps({
  cardText: {
    type: String,
    required: true,
  },
  isInFocus: {
    type: Boolean,
    default: false,
  },
});

const height = (Math.random(Math.floor(Math.random() * 10)) + 1) * 100;
</script>

<style>
.card {
  width: 200px;
  border: gray 1px solid;
  border-radius: 25px;
  margin: 0 auto;
  text-align: center;
  margin-top: 10px;
  margin-bottom: 10px;
}
.selected {
  background-color: #afeeee;
}
</style>
