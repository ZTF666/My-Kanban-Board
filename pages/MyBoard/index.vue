<template>
<div>
<!-- <Particles /> -->
  <div class="top">
  <center>
    <!--  -->
    <v-dialog  v-model="show" persistent max-width="290">
      <Confirmation/>
    </v-dialog> 
    <!--  -->
    <InfoBar />
    <!--  -->
    <v-form @submit.prevent="addTask()">
      <v-row>
        <v-col cols="12" md="12" >
          <v-text-field label="New Task" v-model="task" required color="cyan"></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" md="6" >
          <v-btn type="submit" color="cyan" outlined>Add</v-btn>
        </v-col>
        <v-col cols="12" md="6" >
          <v-btn @click="saveData()" color="success" outlined>Save Session</v-btn>
        </v-col>
      </v-row>
    </v-form>
  </center>


  <v-main transparent >
      <v-container>
        <v-row>
          <v-col>
            <v-sheet min-height="60vh" rounded="lg" >
              <v-container>
                <v-row>
<!-- TODO -->
                  <v-col cols="2.5"  class="mt-5" >
                    <center><h1 class="accent--text">TO DO</h1></center>
                    <v-card flat color="accent" class="text-md-center">
                      <draggable class="list-group" :list="toDo" group="Tasks">
                        <div class="list-group-item" v-for="(element, index) in toDo" :key="index" >
                          {{ element.name }}
                        </div>
                      </draggable>
                    </v-card>
                  </v-col>
<!-- WIP -->
                  <v-col cols="2.5" class="mt-5">
                    <center><h1 class="orange--text">WiP</h1></center>
                    <v-card flat color="orange" class="text-md-center">
                      <draggable class="list-group" :list="wip" group="Tasks">
                        <div class="list-group-item" v-for="(element, index) in wip" :key="index" >
                          {{ element.name }} 
                        </div>
                      </draggable>
                    </v-card>
                  </v-col>
<!-- TO TEST -->
                  <v-col cols="2.5"  class="mt-5">
                    <center><h1 class="purple--text">TO TEST</h1></center>
                    <v-card flat color="purple" class="text-md-center">
                      <draggable v-model="toTest" group="Tasks" >
                        <div class="list-group-item" v-for="(element,index) in toTest" :key="index">{{element.name}}</div>
                      </draggable>
                    </v-card>
                  </v-col>
<!-- REWORK / FIX -->
                  <v-col cols="2.5"  class="mt-5">
                    <center><h1 class="red--text">REWORK</h1></center>
                    <v-card flat color="red" class="text-md-center">
                      <draggable v-model="reworkOrFix" group="Tasks" >
                        <div class="list-group-item" v-for="(element,index) in reworkOrFix" :key="index">{{element.name}}</div>
                      </draggable>
                    </v-card>
                  </v-col>
<!-- DONE -->
                  <v-col cols="2.5"  class="mt-5">
                    <center><h1 class="success--text">DONE</h1></center>
                    <v-card flat color="success" class="text-md-center">
                      <draggable v-model="done" group="Tasks" >
                        <div class="list-group-item" v-for="(element,index) in done" :key="index">{{element.name}}</div>
                      </draggable>
                    </v-card>
                  </v-col>
<!-- end -->
                </v-row>
              </v-container>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
  </v-main>
  </div>


  </div>
</template>

<script>
import db from '~/firebase-config/firestore'
import auth from '~/firebase-config/auth'
import draggable from 'vuedraggable'
import Confirmation from '~/components/Confirmation.vue'
import Particles from '~/components/Particles.vue'
export default {
components:{
    draggable,
    Confirmation,
    Particles
},
data(){
  return{
    task:null,
    show:false,
    toDo:[],
    wip:[],
    toTest:[],
    reworkOrFix:[],
    done:[],
    uid:null
  }
},
methods:{
//ADDING DATA TO LOCAL ARRAY
  addTask(){
    if(this.task){
      this.toDo.push({'name':this.task})
      this.task=null
    }
  },
  saveData(){
//Clears out all the old data then rewrite it so there won't be any duplicate , my knoledge is limited ATM so this will do ... 
let save='save'
   this.AllInOneDelete(save)
   setTimeout(() => {
// To Do
   this.toDo.forEach(e => {
    db.collection("Tasks").doc(this.uid).collection('Todo').add({name:e.name})
    .then(()=>{console.log('done')})})
//WiP
    this.wip.forEach(e=>{
    db.collection("Tasks").doc(this.uid).collection('wip').add({name:e.name})
    .then(()=>{console.log('done')})})
//To Test
    this.toTest.forEach(e=>{
    db.collection("Tasks").doc(this.uid).collection('ToTest').add({name:e.name})
    .then(()=>{console.log('done')})})
//Rework Or Fix
    this.reworkOrFix.forEach(e=>{
    db.collection("Tasks").doc(this.uid).collection('RoF').add({name:e.name})
    .then(()=>{console.log('done')})})
//Done
    this.done.forEach(e=>{
    db.collection("Tasks").doc(this.uid).collection('Done').add({name:e.name})
    .then(()=>{console.log('done')})})
   }, 2000);

  },
  //GETTING DATA
  async getTodos(payload){
    await db.collection("Tasks").doc(payload).collection('Todo').get()
    .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
    this.toDo.push({
      name:doc.data().name,
      id:doc.id
    })})})
    .catch((error) => {
        console.log("Error getting documents: ", error);
    })
  },
  async getWip(payload){
    await db.collection("Tasks").doc(payload).collection('wip').get()
    .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
    this.wip.push({
      name:doc.data().name,
      id:doc.id})})})
    .catch((error) => {
        console.log("Error getting documents: ", error);
    })
  },
  async getRoF(payload){
    await db.collection("Tasks").doc(payload).collection('RoF').get()
    .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
    this.reworkOrFix.push({
      name:doc.data().name,
      id:doc.id})})})
    .catch((error) => {
        console.log("Error getting documents: ", error);
    })
  },
  async getToTest(payload){
    await db.collection("Tasks").doc(payload).collection('ToTest').get()
    .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
    this.toTest.push({
      name:doc.data().name,
      id:doc.id})})})
    .catch((error) => {
        console.log("Error getting documents: ", error);
    })
  },
  async getDone(payload){
    await db.collection("Tasks").doc(payload).collection('Done').get()
    .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
    this.done.push({
      name:doc.data().name,
      id:doc.id})})})
    .catch((error) => {
        console.log("Error getting documents: ", error);
    })
  },
  //DELETING DATA
   deleteTodos(payload){
    db.collection("Tasks").doc(payload).collection("Todo")
    .get()
    .then(res => {
      res.forEach(element => {
        element.ref.delete();
      })
    })
  },
   deleteWip(payload){
    db.collection("Tasks").doc(payload).collection("wip")
    .get()
    .then(res => {
      res.forEach(element => {
        element.ref.delete();
      })
    })
  },
   deleteRoF(payload){
    db.collection("Tasks").doc(payload).collection("RoF")
    .get()
    .then(res => {
      res.forEach(element => {
        element.ref.delete();
      })
    })
  },
   deleteToTest(payload){
    db.collection("Tasks").doc(payload).collection("ToTest")
    .get()
    .then(res => {
      res.forEach(element => {
        element.ref.delete();
      })
    })
  },
   deleteDone(payload){
    db.collection("Tasks").doc(payload).collection("Done")
    .get()
    .then(res => {
      res.forEach(element => {
        element.ref.delete();
      })
    })
  },
  // All In One fetching method that calls all the others
  AllInOneFetch(payload){
    this.getTodos(payload)
    this.getWip(payload)
    this.getToTest(payload)
    this.getRoF(payload)
    this.getDone(payload)
  },
  AllInOneDelete(payload){
    const uid=this.uid
    // onClick Save data to db, deletes everything first , then commit changes (check saveData above)
    if(payload=='save'){
    this.show=false
    // emits events to change alert text and icon (check component InfoBar)
    this.$nuxt.$emit('Save','true')
    this.deleteDone(uid)
    this.deleteRoF(uid)
    this.deleteToTest(uid)
    this.deleteTodos(uid)
    this.deleteWip(uid)
    }
    // onClick new session, deletes everything
    else{
    this.show=false
    // emits events to change alert text and icon (check component InfoBar)
    this.$nuxt.$emit('Delete','true')
    this.deleteDone(uid)
    this.deleteRoF(uid)
    this.deleteToTest(uid)
    this.deleteTodos(uid)
    this.deleteWip(uid)
    setTimeout(() => {
      window.location.reload()
    }, 2000);
    }
  },
},
mounted(){
  // even if the localstorage can be manually created , no writing or updating will happen if the user isn't registered
  // Auto loading all the data from firestore.
  this.uid=localStorage.getItem('uid')
  if(this.uid){
    this.AllInOneFetch(this.uid)
  }
  else{
    location.assign('/')
  }
},
created(){
  // Listening to emitted events .
  // On click new session , shows dialogbox
  this.$nuxt.$on('newSession',()=>{
    this.show=true
  })
  // Canceling the new session event
  this.$nuxt.$on('ConfirmCancel',()=>{
    this.show=false
  })
  // Confirming new session event
  this.$nuxt.$on('ConfirmDelete',()=>{
    this.AllInOneDelete(confirm)
  })
}
}
</script>

<style scoped>
.list-group-item {
  cursor: move;
}
.top{
  margin-top: -80vh;
}

</style>