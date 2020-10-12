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
  background: #07d400;
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
  margin-left: 10px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 6px;
}

button:hover {
  background: #05A900;
}
</style>
