<script setup>
import { Pie } from 'vue-chartjs';
import { Chart as ChartJS, Tooltip, Legend, Title, ArcElement } from 'chart.js';
import ChartDataLabels from 'chartjs-plugin-datalabels';
import { getChartLabelPlugin } from 'chart.js-plugin-labels-dv'

const data = [2280, 732];

ChartJS.register(Tooltip, Legend, Title, ArcElement);

const chartData = { 
    labels: [
        'Билеты',
        'Промокоды',
    ],
    datasets: [ {
        data: data,
        label: 'Доход в сомах',
        backgroundColor: [
            'rgb(252, 53, 95)',
            'rgb(54, 162, 235)',
        ],
        hoverOffset: 8
      } 
    ]
};

const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      datalabels: {
        color: 'rgb(40, 40, 40)',
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
      },
      legend: {
        align: 'end'
      }
    }
};

const chartPlugins = [
  {
    id: 'customCanvasBackgroundColor',
    beforeDraw: (chart, args, options) => {
      const { ctx } = chart;
      ctx.save();
      ctx.globalCompositeOperation = 'destination-over';
      ctx.fillStyle = options.color || '#d9fcf5';
      ctx.fillRect(0, 0, chart.width, chart.height);
      ctx.restore();
    }
  },
  ChartDataLabels,
  getChartLabelPlugin()
];

</script>

<template>
  <div>
    <Pie
        id="pie-chart-1"
        :options="chartOptions"
        :data="chartData"
        :plugins="chartPlugins"
    />
  </div>
</template>
