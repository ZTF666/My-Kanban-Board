<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent  max-width="600px" >
      <v-card flat color="dark" >
        <v-card-title class="justify-center">
          <span class="text-h5 cyan--text ">Change Password</span>
        </v-card-title>
        <ErrorBox :kolor='kolor' :icon='icon' :errorMsg='errorMsg' :alert='alert'/>
        <v-form ref="form" lazy-validation @keyup.native.enter="updatePassword()">
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field label="Email" required v-model="email" :rules="loginRules" ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="Current Password" required 
                    v-model="pwd"
                    :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show ? 'text' : 'password'"
                    @click:append="show = !show"
                    :rules="passwordRules"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="New Password*" required 
                    v-model="newPwd"
                    :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show1 ? 'text' : 'password'"
                    @click:append="show1 = !show1"
                    :rules="passwordRules"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="Confirm New Password*" required
                    v-model="confPwd"
                    :rules="confirmPasswordRules"
                    :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show2 ? 'text' : 'password'"
                    @click:append="show2 = !show2"
                 ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red" text @click="closeModal()" >
            Close
          </v-btn>
          <v-spacer></v-spacer>
         
          <v-btn color="cyan" text @click.prevent="updatePassword()" >
            Update
          </v-btn>
          <v-spacer></v-spacer>
        </v-card-actions>
        </v-form>      

      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import auth from "~/firebase-config/auth"
export default {
    data(){
        return{
            dialog:false,
            email:null,
            pwd:null,
            newPwd:null,
            confPwd:null,
            show: false,
            show1: false,
            show2:false,
            //errorBox props
            kolor:null,
            icon:null,
            errorMsg:null,
            alert:false,
            //
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
    // checks identical password confirmation
    computed: {
    confirmPasswordRules() {
        const rules = [(this.newPwd === this.confPwd)|| "Password must match."];
        return rules;
    },
    },
    methods:{
        // i relog the user then change his password , to bypass that bullshit 5min expiration rule imposed by firebase  
        updatePassword(){
            if(this.$refs.form.validate()){
                auth.signInWithEmailAndPassword(this.email,this.pwd)
                .then((cred=>{
                    cred.user.updatePassword(this.newPwd).then(() => {
                        this.setAlert('green',"Password Changed !",'mdi-check-outline')
                        this.$refs.form.reset()
                        setTimeout(() => {
                            this.dialog=!this.dialog
                        }, 1000);
                }).catch((error) => {
                        this.setAlert('red',error.message,'mdi-alert-circle-outline')
                });
                }))
                .catch(err=>{
                    this.setAlert('red',err.message,'mdi-alert-circle-outline')
                })
                }
                else{
                    this.setAlert('red','Please Fill out the required fields !','mdi-alert-circle-outline')
                }           
            },
        closeModal(){
            this.$refs.form.reset()
            this.dialog=!this.dialog
        },
        setAlert(Z,T,F){
            this.alert= true
            this.kolor=Z
            this.errorMsg=T
            this.icon=F
    }
    },
    // instead of going by emitting events , this can be passed as props from the speed component to this one 
    // i went this route as personal preference nothing more nothing less .
    created(){
        this.$nuxt.$on('togglePChange',()=>{
        this.dialog=true
  })
    }
}
</script>