<template>
  <v-app>
    <v-content>
      <Home />
    </v-content>
    <v-spacer></v-spacer>

    <v-footer>
      <div class="text-center mx-auto">
        &copy; Designed by
        <a
          class="tiya"
          href="https://github.com/alankilalank"
          target="_blank"
        >Alank Ilalank</a>. Developed by
        <a
          class="tiya"
          href="https://github.com/princerafid01"
          target="_blank"
        >Mahmud Rafid</a>.
      </div>
    </v-footer>
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
.tiya {
  text-decoration: none;
  color: rgb(76, 175, 80) !important;
}
</style>