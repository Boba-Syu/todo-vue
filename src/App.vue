<template>
  <p>{{ msg }}</p>
  <card-component></card-component>
</template>

<script setup lang="ts">
import {ref} from "vue";
import CardComponent from "@/components/CardComponent.vue";
const msg = ref('Card Management')
</script>

<style scoped>

</style>

