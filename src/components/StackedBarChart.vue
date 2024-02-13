<script setup>
import { Bar } from 'vue-chartjs';
import ChartDataLabels from 'chartjs-plugin-datalabels';

const DATA_COUNT = 4; // Get from API
const dataLabels = [];
const ticketValues = [140, 145, 211, 123]; 
const promocodeValues = [140, 200, 103, 321]; 

for (var i = 1; i <= DATA_COUNT; ++i) {
  dataLabels.push('Этап ' + i);
}

const roundedCorders = {
              topLeft: 10,
              topRight: 10,
              bottomLeft: 10,
              bottomRight: 10
            };

const chartData = { 
        labels: dataLabels,
        datasets: [ 
          {
            label: 'Билеты',
            data: ticketValues,
            backgroundColor: 'rgb(252, 53, 95)',
            borderColor: 'rgb(255, 255, 255)',
            borderWidth: 1,
            borderRadius: roundedCorders,
            borderSkipped: false
          }, {
            label: 'Промокоды',
            data: promocodeValues,
            backgroundColor: 'rgb(54, 162, 235)',
            borderColor: 'rgba(255, 255, 255)',
            borderWidth: 1,
            borderRadius: roundedCorders,
            borderSkipped: false
          }
        ],
     
    };

const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
        title: {
            display: true,
            text: 'Доход от билетов и промокодов на каждом этапе (в сомах)',
            position: 'bottom',
            font: {
            'size': 15
            }
        },
        legend: {
            align: 'end'
        },
        datalabels: {
          color: 'rgb(40, 40, 40)'
        }
    },
    scales: {
        x: {
            stacked: true,
        },
        y: {
            stacked: true,
            beginAtZero: true,
            grace: 20
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
  }, {
    id: 'topLabels',
    afterDatasetsDraw: (chart, args, options) => {
      const { ctx, scales: { x, y } } = chart;
      chart.data.datasets[0].data.forEach((datapoint, index) => {
        const datasetArray = [];

        chart.data.datasets.forEach((dataset) => {
          datasetArray.push(dataset.data[index]);
        })

        const sum = datasetArray.reduce((acc, val) => acc + val, 0);
        ctx.font = '12px sans-serif';
        ctx.fillStyle = 'rgb(0, 0, 0)';
        ctx.textAlign = 'center';
        ctx.fillText(sum, x.getPixelForValue(index), chart.getDatasetMeta(1).data[index].y - 8);
      })
    }
  },
  ChartDataLabels,
]

</script>

<template>
  <div>
    <Bar
        id="bar-chart-2"
        :options="chartOptions"
        :data="chartData"
        :plugins="chartPlugins"
    />
  </div>
</template>
