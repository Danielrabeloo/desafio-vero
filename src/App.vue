<template>
  <v-app>
    <v-snackbar
      v-if="updateStatus"
      :value="true"
      absolute
      top
      centered
      right
      tile
    >
      {{ updateStatus }}
    </v-snackbar>

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
          <tr v-for="chamada in chamadas" :key="chamada.id">
            <td>{{ chamada.id }}</td>
            <td>{{ chamada.origem }}</td>
            <td>{{ chamada.destino }}</td>
            <td>
              <v-select
                :items="status"
                :value="chamada.estado"
                @change="updateData(chamada.id, $event)"
              ></v-select>
            </td>
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
      chamadas: [],
      status: ["em selecao de fluxo", "chamando", "em curso"],
      updateStatus: null,
    };
  },

  mounted() {
    this.getData();
  },

  methods: {
    getData() {
      axios.get(this.baseURL + "index.php").then((response) => {
        this.chamadas = response.data;
      });

      setTimeout(this.getData, 60000);
    },

    updateData(id, estado) {
      axios
        .get(this.baseURL + "update.php", { params: { id, estado } })
        .then((response) => {
          if (this.updateStatus) {
            this.updateStatus = null;
          }

          if (response.data.status === 200) {
            this.updateStatus = `Sucesso ao atualizar a chamada ${id} (200)`;

            setTimeout(this.removeStatus, 3000);
          } else if (response.data.status === 500) {
            this.updateStatus = `Erro ao atualizar a chamada ${id} (500)`;

            setTimeout(this.removeStatus, 3000);
          }
        });
    },

    removeStatus() {
      this.updateStatus = null;
    },
  },
};
</script>
