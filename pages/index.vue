<template>
<v-card class="zx" flat color="transparent">
      <v-toolbar color="transparent">
        <template v-slot:extension>
          <v-tabs v-model="tabs" centered>
            <v-tab class="green--text">Sign In</v-tab>
            <v-tab class="green--text">Sign Up</v-tab>
          </v-tabs>
        </template>
      </v-toolbar>
      <center>
          <v-img src="https://ztf-shop.web.app/9.png" height="200px" width="200px"></v-img>
      </center>
      <ErrorBox :kolor='kolor' :icon='icon' :errorMsg='errorMsg' :alert='alert'/>
      <v-tabs-items v-model="tabs" class="transparent">
        <v-tab-item class="ma-10 ">
            <div>
                <v-form ref="form" lazy-validation @keyup.native.enter="Emailogin()">
                    <v-text-field label="Email" required :rules="loginRules" v-model="Login" ></v-text-field>
                    <v-text-field label="Password" :rules="passwordRules" :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show1 ? 'text' : 'password'"
                    @click:append="show1 = !show1"
                    v-model="Password" ></v-text-field>
                    <center>
                    <v-btn color="green"  @click.prevent="Emailogin()">Sign In</v-btn>              
                    </center>
                </v-form>
            </div>
        </v-tab-item>
        <v-tab-item class="ma-10">
            <div>
                <v-form ref="formUp" lazy-validation @keyup.native.enter="Register()">
                    <v-text-field label="Email" required :rules="loginRules" v-model="Login" ></v-text-field>
                    <v-text-field label="Password" :rules="passwordRules" :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show1 ? 'text' : 'password'"
                    @click:append="show1 = !show1"
                    v-model="Password" ></v-text-field>
                    <center>
                    <v-btn color="green"  @click.prevent="Register()">Sign Up</v-btn>              
                    </center>
                </v-form>
            </div>
        </v-tab-item>
      </v-tabs-items>
    </v-card>
</template>

<script>
import auth from "~/firebase-config/auth"
export default {
data(){
    return{
        //errorBox props
        kolor:null,
        icon:null,
        errorMsg:null,
        alert:false,
        //
        Login:null,
        Password:null,
        tabs:null,
        show1: false,
        loginRules: [
            (v) => !!v || 'E-mail is required',
            (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],
        passwordRules: [
            (v) => !!v || 'Password is required',
            (v) => (v && v.length >= 6) || 'Password must be 6 or more characters',
        ],
    }
},
methods:{
    Emailogin(){
        if(this.$refs.form.validate()){
        auth.signInWithEmailAndPassword(this.Login,this.Password)
        .then((cred=>{
            localStorage.setItem('uid',cred.user.uid)
            location.assign('/MyBoard/')
        }))
        .catch(err=>{
            this.setAlert('red',err.message,'mdi-alert-circle-outline')
        })
        }
        else{ 
            this.setAlert('orange','Please Fill the required fields !','mdi-alert-circle-outline')
         }
    },
    Register(){
        if(this.$refs.formUp.validate()){
            auth.createUserWithEmailAndPassword(this.Login,this.Password)
            .then((cred)=>{
            localStorage.setItem('uid',cred.user.uid)
            location.assign('/MyBoard/')
                    })
            .catch((error) => {
                this.setAlert('red',error.message,'mdi-alert-circle-outline')
            })
            }
    },
    setAlert(Z,T,F){
        this.alert= true
        this.kolor=Z
        this.errorMsg=T
        this.icon=F
    }
}}
</script>
<style scoped>
.zx{
    z-index: 2;
}
</style>