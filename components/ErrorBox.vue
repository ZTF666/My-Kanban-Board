<template>
  <v-container>
  <v-alert class="mt-10" 
  v-model="alert"
  :color="kolor"
  dense 
  :icon="icon" 
  prominent border="left" >
  {{errorMsg}}
  </v-alert>  
</v-container>
</template>

<script>
export default {
    props:['kolor','icon','errorMsg','alert']
}
</script>
