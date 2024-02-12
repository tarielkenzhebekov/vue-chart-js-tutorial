<script setup>
import { Pie } from 'vue-chartjs';
import { Chart as ChartJS, Tooltip, Legend, Title, ArcElement } from 'chart.js';
import ChartDataLabels from 'chartjs-plugin-datalabels';
import { ref, computed } from 'vue';

const dynamicData = ref([150, 50]);

const onClickDo = () => {
  dynamicData.value = [dynamicData.value[0] + 1, dynamicData.value[1] - 1];
}

ChartJS.register(Tooltip, Legend, Title, ArcElement, ChartDataLabels);

const chartData = computed(() => {
    return { 
        labels: [
            'Билеты',
            'Промокоды',
        ],
        datasets: [ {
            data: dynamicData.value,
            label: 'В процентах',
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
        anchor: 'end',
        align: 'end',
        formatter: ((value, ctx) => {
          const totalSum = ctx.dataset.data.reduce((accumulator, currentValue) => {
            return accumulator + currentValue;
          }, 0);
          console.log(totalSum);
          const percentage = value / totalSum * 100;
          return `${percentage.toFixed(1)}%`;
        })
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
        id="my-chart-id"
        :options="chartOptions"
        :data="chartData"
    />
  </div>
  <button v-on:click="onClickDo">Click me</button>
</template>
