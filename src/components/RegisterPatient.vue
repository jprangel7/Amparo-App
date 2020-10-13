<template>
  <v-dialog v-model="show" max-width="600px">
    <v-card>
      <h4 class="modal-title">Novo Paciente</h4>
      <div class="input-container">
        <input class="input" v-model="name" placeholder="Nome" />
        <input class="input" v-model="cpf" placeholder="CPF" />
      </div>
      <v-card-actions>
        <div class="modal-btns"></div>
        <v-btn @click.stop="registerPatient">Cadastrar</v-btn>
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
      name: "",
      cpf: "",
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

  methods: {
    async registerPatient() {
      const newPatient = {
        name: this.name,
        cpf: this.cpf,
      };
      await api
        .post("/patient", newPatient)
        .then((response) => console.log(response.data))
        .catch((err) => console.log(err));

      this.name = "";
      this.cpf = "";
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

.modal-btns {
  margin: 30px 0px 30px auto;
}

.input-container {
  display: flex !important;
  align-items: center;
  justify-content: center;
}
</style>
