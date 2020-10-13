<template>
  <v-dialog v-model="show" max-width="650px">
    <v-card>
      <h4 class="modal-title">Nova Atividade</h4>
      <div>
        <v-row class="center">
          <v-col cols="10">
            <v-autocomplete
              v-model="select"
              :loading="loading"
              :items="items"
              :search-input.sync="search"
              cache-items
              outlined
              hide-no-data
              placeholder="Nome do Paciente"
              item-text="name"
              return-object
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row class="center">
          <v-col cols="5">
            <v-menu
              v-model="menu"
              :close-on-content-click="false"
              :nudge-right="40"
              transition="scale-transition"
              offset-y
              min-width="290px"
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="date"
                  placeholder="Data de Validade"
                  readonly
                  outlined
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="date"
                @input="menu = false"
              ></v-date-picker>
            </v-menu>
          </v-col>
          <v-col cols="5">
            <v-select
              outlined
              :items="status"
              v-model="selectedStatus"
              placeholder="Status"
            ></v-select>
          </v-col>
        </v-row>
        <v-row class="center">
          <v-col cols="10">
            <v-textarea
              outlined
              class="textarea"
              no-resize
              v-model="activity"
              placeholder="Atividade"
            />
          </v-col>
        </v-row>
      </div>
      <v-card-actions>
        <div class="modal-btns"></div>
        <v-btn @click.stop="registerActivity">Cadastrar</v-btn>
        <v-btn class="close" @click.stop="show = false">Fechar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import api from "../services/api.js";

export default {
  data() {
    return {
      patients: [],
      items: [],
      activity: null,
      date: null,
      selectedStatus: null,
      loading: true,
      search: null,
      select: null,
      status: ["Aberto", "Atrazado", "Finalizado"],
      menu: false,
    };
  },

  props: {
    value: Boolean,
  },

  computed: {
    show: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit("input", value);
      },
    },
  },
  watch: {
    search(val) {
      val && val !== this.select && this.querySelections(val);
    },
  },
  methods: {
    async registerActivity() {
      const newActivity = {
        patient: this.select,
        expiration_date: this.date,
        status: this.selectedStatus,
        activity: this.activity,
      };
      await api
        .post("/activity/newActivity", newActivity)
        .then((response) => {
          console.log(response.data);
          this.$emit("add-activity", response.data);
        })
        .catch((err) => console.log(err));

      this.activity = null;
      this.date = null;
      this.selectedStatus = null;
      this.select = null;
    },
    async querySelections() {
      this.loading = true;
      await api
        .get("/patient")
        .then((response) => {
          this.patients = response.data;
          this.items = this.patients.filter((e) => {
            return e;
          });
          this.loading = false;
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
.modal-title {
  font-size: 20px;
  padding: 15px;
}

.close {
  background: #b10000 !important;
  margin-right: 10px;
}

.close:hover {
  background: #7e0000 !important;
}

.modal-btns {
  margin: 30px 0px 30px auto;
}

.center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.v-dialog {
  overflow-y: unset !important;
}
</style>
