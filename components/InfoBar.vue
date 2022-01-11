<template>
  <v-alert
  class="mb-10"
      color="dark"
      dark
      dense
      :icon="icon"
      prominent
      border="left"
      dismissible
    >
      {{InfoText}}
    </v-alert>
</template>

<script>
export default {
data(){
    return{
        InfoText:'Make sure to save your session before exiting or you will lose your data .',
        icon:'mdi-information'
    }
},
created(){
// ON Deleting data message & icon 
this.$nuxt.$on('Delete',()=>{
  this.InfoText='DELETING DATA PLEASE WAIT ...'
  this.icon='mdi-delete-sweep'
})
// ON Saving data message & icon
this.$nuxt.$on('Save',()=>{
  this.InfoText='Saving DATA PLEASE WAIT ...'
  this.icon='mdi-content-save'
  // This 2000ms is based on my shitty internet , it can be reworked withtout a timer
  // you can emit an event when all the data is saved then change the infotext
  setTimeout(() => {
    this.InfoText='Data Saved !'
  }, 2000);
})


}
}
</script>

<style>

</style>