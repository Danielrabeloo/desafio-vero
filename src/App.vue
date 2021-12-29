<template>
  <v-app>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Id</th>
            <th class="text-left">Origem</th>
            <th class="text-left">Destino</th>
            <th class="text-left">Estado</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.origem }}</td>
            <td>{{ item.destino }}</td>
            <td>{{ item.estado }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      baseURL: "http://191.252.93.122/desafio-front-end/api/",
      items: [],
    };
  },

  mounted() {
    this.getData();
  },

  methods: {
    getData() {
      axios.get(this.baseURL + "/index.php").then((response) => {
        this.items = response.data;
      });

      setTimeout(this.getData, 6000000);
    },
  },
};
</script>
