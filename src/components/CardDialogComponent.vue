<script setup lang="ts">

import {ref} from "vue";
import {useDraggable} from "vue-draggable-plus";

const props = defineProps([
  'subCard'
])

const el = ref()
const draggableOption = {
  animation: 150,
  ghostClass: 'ghost',
  onStart() {
  },
  onUpdate() {
  }
}
useDraggable(el, props.subCard.subCardItems, draggableOption)
</script>

<template>
  <el-card class="box-card" style="width: 300px">
    <template #header>
      <div class="card-header">
        <span>{{ props.subCard.subCardName }}</span>
      </div>
    </template>
    <div class="text item" ref="el">
      <p v-for="subCardItem in props.subCard.subCardItems" :key="subCardItem">
        {{ subCardItem }}
      </p>
    </div>
  </el-card>
</template>

<style scoped>

</style>