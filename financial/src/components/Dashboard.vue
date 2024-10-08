<template>
  <div
    class="h-screen mx-auto p-4 bg-white dark:bg-[#313131] shadow-md transition-colors"
  >
    <!-- Botão de Dark Mode -->
    <div class="flex justify-end mb-4">
      <button
        @click="toggleDarkMode"
        class="px-4 py-2 bg-gray-300 dark:bg-gray-600 text-black dark:text-white rounded"
      >
        {{ darkMode ? "Modo Claro" : "Modo Escuro" }}
      </button>
    </div>

    <h1 class="text-2xl font-bold mb-4 text-black dark:text-white">
      Dashboard Financeiro
    </h1>

    <!-- Tabela de Registros -->
    <table
      class="min-w-full table-auto border-collapse border border-gray-300 dark:border-gray-600 mb-6"
    >
      <thead class="bg-gray-100 dark:bg-gray-700">
        <tr>
          <th
            class="border border-gray-300 dark:border-gray-600 px-4 py-2 text-black dark:text-white"
          >
            Descrição
          </th>
          <th
            class="border border-gray-300 dark:border-gray-600 px-4 py-2 text-black dark:text-white"
          >
            Tipo
          </th>
          <th
            class="border border-gray-300 dark:border-gray-600 px-4 py-2 text-black dark:text-white"
          >
            Valor (R$)
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(registro, index) in registros"
          :key="index"
          class="text-center"
        >
          <td
            class="border border-gray-300 dark:border-gray-600 px-4 py-2 text-black dark:text-white"
          >
            {{ registro.descricao }}
          </td>
          <td
            class="border border-gray-300 dark:border-gray-600 px-4 py-2 text-black dark:text-white"
          >
            {{ registro.tipo }}
          </td>
          <td
            :class="
              registro.tipo === 'Entrada' ? 'text-green-500' : 'text-red-500'
            "
            class="border border-gray-300 dark:border-gray-600 px-4 py-2"
          >
            {{ registro.valor.toFixed(2) }}
          </td>
        </tr>
      </tbody>
    </table>

    <div class="mt-6 flex justify-end">
      <span class="text-lg font-bold text-black dark:text-white">
        Saldo:
        <span :class="saldo >= 0 ? 'text-green-500' : 'text-red-500'">{{
          saldo.toFixed(2)
        }}</span>
        R$
      </span>
    </div>

    <!-- Verifique se chartData está disponível antes de renderizar o gráfico -->
    <div v-if="chartData">
      <FinancialChart :chart-data="chartData" />
    </div>

  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import FinancialChart from "./FinancialChart.vue"; // Importa o componente de gráfico

const registros = ref([
  { descricao: "Salário", tipo: "Entrada", valor: 3000 },
  { descricao: "Aluguel", tipo: "Saída", valor: -1000 },
  { descricao: "Freelance", tipo: "Entrada", valor: 1000 },
  { descricao: "Supermercado", tipo: "Saída", valor: -1000 },
  { descricao: "Farmacia", tipo: "Saída", valor: -300 },
]);

const saldo = computed(() => {
  return registros.value.reduce((acc, registro) => acc + registro.valor, 0);
});

// Verifique se há registros disponíveis antes de gerar os dados do gráfico
const chartData = computed(() => {
  if (!registros.value.length) return null; // Se não houver registros, retorne null

  const labels = registros.value.map((r) => r.descricao);
  const valores = registros.value.map((r) => r.valor);
  const backgroundColors = registros.value.map((r) =>
    r.tipo === "Entrada" ? "rgba(75, 192, 192, 0.6)" : "rgba(255, 99, 132, 0.6)"
  );

  return {
    labels,
    datasets: [
      {
        label: "Valores Financeiros",
        data: valores,
        backgroundColor: backgroundColors,
        borderColor: backgroundColors.map((color) => color.replace("0.6", "1")),
        borderWidth: 1,
      },
    ],
  };
});

// Controle do modo escuro
const darkMode = ref(false);

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
  document.documentElement.classList.toggle("dark", darkMode.value);
};

// Detectar tema inicial com base na preferência do usuário
onMounted(() => {
  darkMode.value = window.matchMedia("(prefers-color-scheme: dark)").matches;
  document.documentElement.classList.toggle("dark", darkMode.value);
});
</script>

<style scoped>
/* Adicione algum estilo extra, se necessário */
</style>
