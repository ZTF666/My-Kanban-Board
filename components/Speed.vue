<template>
<div>
    <v-speed-dial v-model="fab" bottom right class="pos">
      <template v-slot:activator>
        <v-btn v-model="fab" color="red"  fab >
          <v-icon v-if="fab"> mdi-close </v-icon>
          <v-icon v-else>  mdi-account-circle </v-icon>
        </v-btn>
      </template>
      <v-btn fab dark small color="transparent" @click="$nuxt.$emit('toggleOpacity','true')">
        <v-icon>mdi-circle-opacity</v-icon>
      </v-btn>
      <v-btn fab dark small color="transparent" @click="$nuxt.$emit('togglePChange','true')">
        <v-icon>mdi-form-textbox-password</v-icon>
      </v-btn>
    </v-speed-dial>
    <!-- Change password component -->
    <ChangePassword  />
</div>


</template>
<script>
  export default {
    data: () => ({
      fab: false,
    }),
  }
</script>
<style>
  .pos{
      position: fixed;
  }
</style>