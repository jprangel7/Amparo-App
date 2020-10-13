<template>
  <div class="table-container">
    <v-data-table
      :headers="headers"
      :items="activities"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.status="{ item }">
        <v-select
          v-model="item.status"
          :items="statusList"
          @change="updateStatus(item)"
        ></v-select>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import api from "../services/api.js";

export default {
  name: "Activities",

  data: () => ({
    headers: [
      {
        text: "Paciente",
        align: "start",
        sortable: true,
        value: "patient_id.name",
        width: "17%",
      },
      { text: "CPF", value: "patient_id.cpf", width: "13%" },
      { text: "Data", value: "expiration_date", width: "13%" },
      { text: "Atividade", value: "activity" },
      { text: "Status", value: "status", width: "15%" },
    ],
    statusList: ["Aberto", "Atrasado", "Finalizado"],
    loading: true,
  }),

  props: ["activities"],

  methods: {
    async updateStatus(item) {
      const data = {
        id: item.id,
        status: item.status,
      };
      this.loading = true;
      await api
        .put("/activity", data)
        .then((response) => {
          console.log(response);
          this.loading = false;
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>
<style>
.table-container {
  margin-top: 20px;
}

th {
  text-align: start;
  font-size: 19px !important;
}
</style>
