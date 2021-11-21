<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <h1>Veggies Inventory</h1>
    </v-app-bar>

    <v-main>
      <DataTable/>
    </v-main>
  </v-app>
</template>

<script>
import DataTable from './components/DataTable.vue';

export default {
  name: 'App',

  components: {
    DataTable,
  },

  data: () => ({
    //
  }),
};
</script>
