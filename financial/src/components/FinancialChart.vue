<template>
  <div v-if="chartData">
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup>
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

// Registrar componentes para Chart.js
ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

// Receber os dados do gráfico via props
const props = defineProps({
  chartData: {
    type: Object,
    required: true,
  },
});

// Configuração do gráfico de barras
const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  scales: {
    y: {
      beginAtZero: true,
    },
  },
};
</script>

<style scoped>
/* Defina a altura do gráfico */
div {
  height: 400px;
}
</style>
