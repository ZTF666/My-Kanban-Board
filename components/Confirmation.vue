<template>
      <v-card  >
        <v-card-title class="text-h5">
          Are you sure ?
        </v-card-title>
        <v-card-text>Pressing <b style="color:rgb(233, 58, 58);">YES</b> will delete all your data permanently from the database !</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn  color="green darken-1" text @click="$nuxt.$emit('ConfirmCancel','true')">
            NO
          </v-btn>
          <v-btn color="red darken-1" text @click="$nuxt.$emit('ConfirmDelete','true')" >
            YES
          </v-btn>
        </v-card-actions>
      </v-card>    
</template>

<script>
export default {
data(){
    return{
       
    }
}
}
</script>

<style>

</style>