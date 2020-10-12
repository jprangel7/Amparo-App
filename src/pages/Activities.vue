<template>
  <div>
    <div class="header-container">
      <img src="../assets/logotipo-amparo-white.png" class="amparo-logo" />
    </div>
    <div class="create-container">
      <p>Lista de Atividades</p>
      <div>
        <button v-on:click="registerPatient">Novo Paciente</button>
        <button v-on:click="registerActivity">Nova Atividade</button>
      </div>
    </div>

    <modal v-if="showModal" @close="showModal = false"> fon </modal>
  </div>
</template>

<script>
// import api from "../services/api.js";

export default {
  name: "Activities",

  components: {},

  data: () => ({
    showModal: false,
  }),

  methods: {
    registerPatient() {
      this.showModal = true;
    },

    registerActivity() {
      console.log("dale");
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
  font-weight: bold;
}

.create-container div {
  margin-left: auto;
}

.amparo-logo {
  width: 180px;
}
</style>
