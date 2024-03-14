<script setup>
import Header from "./components/Header.vue";
import DataTable from "./components/DataTable.vue";
</script>

<template>
  <v-app>
    <Header />

    <v-container class="mt-15">
      <v-col>
        <DataTable />
      </v-col>
    </v-container>
  </v-app>
</template>
