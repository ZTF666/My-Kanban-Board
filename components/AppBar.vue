<template>
  <v-container>
   <v-app-bar :clipped-left="clipped" fixed app dense color="black zztf--text">
      <v-img
        alt="Vuetify Logo"
        class="shrink mr-2 img"
        contain
        :src="logo"
        transition="scale-transition"
        width="40"
      />
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn text class="zztf--text" v-show="logged" @click="$nuxt.$emit('newSession','true')" >New Session</v-btn>
      <v-btn text class="zztf--text" v-show="logged" @click="logout()" >Logout</v-btn>
      <v-btn text class="zztf--text"  :href="link" target="_blank" ><v-icon>mdi-github</v-icon></v-btn>
      <v-btn text class="zztf--text"  :href="AboutMe" target="_blank" >About Me</v-btn>
    </v-app-bar>
  </v-container>
</template>

<script>
import auth from "~/firebase-config/auth";
export default {
  data(){
    return{
      logo:'https://ztf-shop.web.app/9.png',
      title:'My Kanban Board',
      clipped: false,
      link:'https://github.com/ZTF666/My-Kanban-Board',
      AboutMe:'https://ztfportfolio.web.app/',
      logged:false,
    }
  },
  methods:{
    logout(){
      auth.signOut().then(() => {
        // Sign-out successful.
      localStorage.removeItem('uid')
      location.assign('/')
      }).catch((error) => {
        console.log(error)
      })
    }
    },
    mounted(){
      if(localStorage.getItem('uid')){
        this.logged = !this.logged
      }else{
        this.logged=false  
      }
    }
}
</script>

<style>
</style>