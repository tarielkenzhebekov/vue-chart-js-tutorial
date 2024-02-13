<script setup>
import PieChart from './components/PieChart.vue';
import BarChart from './components/BarChart.vue';
import StackedBarChart from './components/StackedBarChart.vue';

</script>

<template>
  <div class="grid-container">
    <div class="grid-item">
      <PieChart>Chart couldn't be loaded.</PieChart>
      <BarChart>Chart couldn't be loaded.</BarChart>
    </div>
    <div class="grid-item">
      <StackedBarChart>Chart couldn't be loaded.</StackedBarChart>
    </div>
  </div>
</template>

<style scoped>
.grid-container {
  width:100%;
  margin-right:10px;
  margin-left:10px;
  display: grid;
  grid-template-columns: auto auto;
  gap: 10px;
}

.grid-item {
  padding-top:30px;
  padding-bottom:30px;
  display: grid;
  grid-template-columns: auto;
  align-content: flex-start;
  gap:25px;
  border: solid 1px;
}

.grid-item div {
  width: 500px;
  height: 330px;
  margin-left: auto;
  margin-right: auto;
}
</style>
