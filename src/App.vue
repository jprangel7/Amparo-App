<template>
  <div id="app">
    <Activities />
  </div>
</template>

<script>
import Activities from "./pages/Activities.vue";

export default {
  name: "App",
  components: {
    Activities,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  outline: 0;
  box-sizing: border-box;
}

button {
  display: inline-block;
  border: none;
  background: #147875 !important;
  color: #fff !important;
  padding: 10px 20px;
  cursor: pointer;
  margin-left: 10px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 6px;
}

button:hover {
  background: #093635 !important;
}

.input {
  height: 40px;
  border: 1px solid #848484;
  border-radius: 3px;
  margin: 10px 10px 10px 10px;
  padding: 10px;
}
</style>
