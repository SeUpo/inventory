<template>
  <div class="search-panel" v-show="show">
    <img class="btn" @click="show = false" src="../assets/Vector.svg" alt="x" />
    <div class="skeleton" style="margin-left: 16px; width: 699px; height: 36px">
      <div class="loading" style="width: 800px"></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
let show = ref(true)
</script>

<style lang="scss" scoped>
.search-panel {
  position: relative;
  display: grid;
  align-items: center;
  border-radius: 10px;
  border: 1px solid $border-grey;
  width: 785px;
  height: 72px;
  background-color: $base-grey;
  .btn {
    position: absolute;
    top: 14px;
    right: 14px;
    cursor: pointer;
  }
}
</style>
