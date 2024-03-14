<template>
  <v-card>
    <v-card-title>
      Nitido
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        label="Buscar"
        @input="filterTable"
        hide-details
        single-line
        append-icon="search"
      ></v-text-field>
    </v-card-title>
    <v-data-table :items="filteredItems" :search="search" :headers="headers" style="background-color: #f5f5f5;"></v-data-table>
  </v-card>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import axios from "axios";

const items = ref([]);
const search = ref("");
const filteredItems = ref([]);


const headers = [
  { title: 'Arrangement ID', key: 'arragmentId' },
  { title: 'Customer Number', key: 'CustomerNumber' },
  { title: 'Loan Type', key: 'loanType' },
];

const loadData = async () => {
  try {
    const response = await axios.get("http://localhost:3000/consumer");
    items.value = response.data;
    filteredItems.value = response.data;
  } catch (error) {
    console.error("Error al cargar los datos:", error);
  }
};

const filterTable = () => {
  // Filtra los items en base al término de búsqueda
  filteredItems.value = items.value.filter((item) =>
    Object.values(item).some((value) =>
      value.toString().toLowerCase().includes(search.value.toLowerCase())
    )
  );
};

onMounted(loadData);

// Vuelve a filtrar cuando los items o el término de búsqueda cambian
watch([items, search], filterTable);
</script>
