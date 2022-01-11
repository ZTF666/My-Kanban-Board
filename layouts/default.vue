<template>
  <v-app dark>
    <AppBar/>
    <v-main>
      <v-container>
    <Particles />

        <Nuxt />
      </v-container>
    </v-main>

    <v-footer absolute app color="transparent">
      <v-spacer></v-spacer><span>&copy; {{ new Date().getFullYear() }} | By : <strong>ZTF</strong></span><v-spacer></v-spacer>
    </v-footer>
  </v-app>
</template>

<script>
import Particles from "../components/Particles.vue";
export default {
    data() {
        return {};
    },
    components: { Particles }
}
</script>
