<template>
  <v-app>
    <v-content>
      <Home />
    </v-content>
  </v-app>
</template>

<script>
import Home from "./components/Home";

export default {
  name: "App",

  components: {
    Home
  },

  data: () => ({
    //
  })
};
</script>
<style>
#app {
  background: rgb(244, 246, 248) none repeat scroll 0% 0% !important;
  font-family: "Roboto", sans-serif;
}
</style>