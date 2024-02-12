<script setup>
import { Pie } from 'vue-chartjs';
import { Chart as ChartJS, Tooltip, Legend, Title, ArcElement } from 'chart.js';
import ChartDataLabels from 'chartjs-plugin-datalabels';
import { getChartLabelPlugin } from 'chart.js-plugin-labels-dv'
import { ref, computed } from 'vue';

const dynamicData = ref([2284, 732]);

const onClickDo = () => {
  dynamicData.value = [dynamicData.value[0] + 1, dynamicData.value[1] - 1];
}

ChartJS.register(Tooltip, Legend, Title, ArcElement, getChartLabelPlugin(), ChartDataLabels);

const chartData = computed(() => {
    return { 
        labels: [
            'Билеты',
            'Промокоды',
        ],
        datasets: [ {
            data: dynamicData.value,
            label: 'Доход в сомах',
            backgroundColor: [
                'rgb(255, 99, 132)',
                'rgb(54, 162, 235)',
            ],
            hoverOffset: 8
         } ]
    }

});

const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      datalabels: {
        color: 'black',
        formatter: ((value, ctx) => {
          return value;
        })
      },
      labels: {
        render: 'percentage',
        precision: 1,
        position: 'outside',
        textMargin: 6
      },
      title: {
        display: true,
        text: 'Доход от билетов и промокодов за весь период (в сомах)',
        position: 'bottom',
        font: {
          'size': 15
        }
      }
    }
};

</script>

<template>
  <div style="width:500px">
    <Pie
        id="bar-chart-1"
        :options="chartOptions"
        :data="chartData"
    />
  </div>
  <button v-on:click="onClickDo">Click me</button>
</template>
