<template>
  <main class="container">
    <div class="section-2fr">
      <VDashboard/>
      <VInventory/>
    </div>
    <div class="section">
      <VSearch/>
    </div>
  </main> 
</template>

<script setup>
import VDashboard from './components/VDashboard.vue'
import VInventory from './components/VInventory.vue'
import VSearch from './components/VSearch.vue'
</script>

<style lang="scss" scoped>
.container {
  max-width: 1280px;
  margin: 0 auto;
}
  .section-2fr {
    margin: 0 auto;
    display: grid;
    gap: 24px;
    grid-template-columns: 236px 525px;
    margin-bottom: 24px;
  }
</style>
