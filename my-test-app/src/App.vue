<template>
  <div id="app">
    <ButtonField />
  </div>
</template>

<script>
import ButtonField from "./components/ButtonField.vue";

export default {
  name: "App",
  components: {
    ButtonField,
  },
};
</script>

<style>
body {
  background-color: lightgray;
  overflow: hidden;
}
</style>
