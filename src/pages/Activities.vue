<template>
  <div data-app>
    <div class="header-container">
      <img src="../assets/logotipo-amparo-white.png" class="amparo-logo" />
    </div>
    <div class="create-container">
      <p>Lista de Atividades</p>
      <div>
        <button v-on:click="showRegisterPatientModal = true">
          Novo Paciente
        </button>
        <button v-on:click="showRegisterActivityModal = true">
          Nova Atividade
        </button>
      </div>
    </div>
    <div>
      <div>
        <v-row>
          <v-col cols="3" style="margin-left: 50px"
            ><v-text-field
              placeholder="CPF do Paciente"
              outlined
              v-model="cpfFilter"
          /></v-col>
          <v-col cols="3">
            <v-select
              outlined
              :items="statusList"
              v-model="statusFilter"
              placeholder="Status do Aprazamento"
            ></v-select>
          </v-col>
          <v-col cols="3"
            ><v-menu
              v-model="menu"
              :close-on-content-click="false"
              :nudge-right="40"
              transition="scale-transition"
              offset-y
              min-width="290px"
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="dateFilter"
                  placeholder="Data"
                  readonly
                  outlined
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="dateFilter"
                @input="menu = false"
              ></v-date-picker> </v-menu
          ></v-col>
          <v-col cols="2">
            <button style="height:56px" @click="applyFilters">
              Filtrar
            </button>
          </v-col>
        </v-row>
      </div>
      <ActivityTable v-bind:activities="activities" />
    </div>

    <RegisterPatient v-model="showRegisterPatientModal" />
    <RegisterActivity
      v-on:add-activity="addActivity"
      v-model="showRegisterActivityModal"
    />
  </div>
</template>

<script>
import { format, parseISO } from "date-fns";

import api from "../services/api.js";

import RegisterPatient from "../components/RegisterPatient.vue";
import RegisterActivity from "../components/RegisterActivity.vue";
import ActivityTable from "../components/ActivityTable.vue";

export default {
  name: "Activities",

  components: {
    RegisterPatient,
    RegisterActivity,
    ActivityTable,
  },

  data: () => ({
    showRegisterPatientModal: false,
    showRegisterActivityModal: false,
    activities: [],
    cpfFilter: null,
    statusFilter: null,
    dateFilter: null,
    menu: false,
    statusList: ["Aberto", "Atrasado", "Finalizado"],
  }),

  async created() {
    await api
      .get("/activity")
      .then((response) => {
        this.activities = response.data;
        this.activities.forEach((activity) => {
          activity.expiration_date = this.formatDate(activity.expiration_date);
        });
        this.loading = false;
      })
      .catch((err) => console.log(err));
  },

  methods: {
    addActivity(newActivity) {
      this.activities = [...this.activities, newActivity];
    },
    formatDate(date) {
      return format(parseISO(date), "dd/MM/yyyy");
    },
    async applyFilters() {
      await api
        .get("/activity")
        .then((response) => {
          this.activities = response.data;
          this.activities.forEach((activity) => {
            activity.expiration_date = this.formatDate(
              activity.expiration_date
            );
          });
          this.loading = false;
        })
        .catch((err) => console.log(err));

      if (this.cpfFilter) {
        this.activities = this.activities.filter((i) => {
          return i.patient_id.cpf === this.cpfFilter;
        });
      }
      if (this.statusFilter) {
        this.activities = this.activities.filter((i) => {
          return i.status === this.statusFilter;
        });
      }
      if (this.dateFilter) {
        this.activities = this.activities.filter((i) => {
          return i.expiration_date === this.formatDate(this.dateFilter);
        });
      }

      this.cpfFilter = null;
      this.statusFilter = null;
      this.dateFilter = null;
    },
  },
};
</script>

<style>
.header-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80px;
  color: white;
  background-color: #093635;
  font-size: 18px;
}

.create-container {
  display: flex;
  align-items: center;
  height: 70px;
  padding: 20px;
  background-color: #c9c9c9;
  font-size: 20px;
  font-weight: bold;
}

.create-container div {
  margin-left: auto;
}

.amparo-logo {
  width: 180px;
}
</style>
