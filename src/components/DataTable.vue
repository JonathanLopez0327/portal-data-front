<template>
  <v-card class="elevation-1 mx-auto">
    <v-card-title class="d-flex align-center pe-2">
      Data
      <v-spacer></v-spacer>
      <v-text-field v-model="search" label="Buscar" @input="filterTable" density="compact"
        prepend-inner-icon="mdi-magnify" variant="solo-filled" flat hide-details single-line></v-text-field>
    </v-card-title>

    <v-data-table :headers="headers" :items="filteredData" :items-per-page="5" class="elevation-1">
      <template v-slot:item.actions="{ item }">
        <v-btn @click="openModal(item.payments)" density="comfortable" icon="mdi-eye-outline" color="#0D5E0B"></v-btn>
      </template>
    </v-data-table>

    <v-dialog v-model="modalVisible">
      <v-card class="mx-auto">
        <v-card-title>Pagos</v-card-title>
        <v-card-text>
          <v-list>
            <v-list-item v-for="(payment, index) in selectedPayments" :key="index">
              <v-list-item-content>
                <v-list-item-title>{{ payment.psPaymentType }}</v-list-item-title>
                <v-list-item-subtitle>psPerCalc: {{ payment.psPerCalc }}</v-list-item-subtitle>
                <v-list-item-subtitle>psPayProp: {{ payment.psPayProp }}</v-list-item-subtitle>
                <v-list-item-subtitle>psPayFreq: {{ payment.psPayFreq }}</v-list-item-subtitle>
                <v-list-item-subtitle>psPaymentMethod: {{ payment.psPaymentMethod }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
        <v-card-actions>
          <v-btn color="red" @click="modalVisible = false" class="ma-2">
            <v-icon icon="mdi-minus-circle" start></v-icon>
            Cerrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      headers: [
        { title: 'ID', value: 'id' },
        { title: 'AA232020DX1G', value: 'AA232020DX1G' },
        { title: 'DOP', value: 'DOP' },
        { title: 'Categoria', value: 'category' },
        { title: 'Cur', value: 'CUR' },
        { title: 'Pagos', value: 'actions', sortable: false }
      ],
      data: [],
      modalVisible: false,
      selectedPayments: [],
      search: '',
      filteredData: [] // Inicializa filteredData aquí
    };
  },
  created() {
    this.loadData();
  },
  methods: {
    async loadData() {
      try {
        const response = await axios.get('http://localhost:3000/data');
        this.data = response.data;
        this.filteredData = response.data; // Asigna los datos recibidos a filteredData
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    openModal(payments) {
      this.selectedPayments = payments;
      this.modalVisible = true;
    },
    filterTable() {
      const searchText = this.search.toLowerCase();
      this.filteredData = this.data.filter(item => {
        return Object.values(item).some(val => {
          if (Array.isArray(val)) {
            return val.some(payment => Object.values(payment).some(subVal => String(subVal).toLowerCase().includes(searchText)));
          }
          return String(val).toLowerCase().includes(searchText);
        });
      });
    }
  }
};
</script>
