<template>
    <div>
        <v-app>
            <div id="login">
                <v-container id="container">
                    <v-row>
                        <v-col>
                            <v-img src="../assets/Logo.png" max-height="250" max-width="250" class="justify-center"></v-img>
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col>
                            <v-text-field v-model="username" solo label="Username" clearable></v-text-field>
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col>
                            <v-text-field v-model="password" solo label="Passwort" clearable type="password"></v-text-field>
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col>
                            <v-btn color="primary" @click="login(username, password)">Login</v-btn>
                        </v-col>
                    </v-row>
                </v-container>
            </div>
        </v-app>
    </div>
</template>

<script>
import sha256 from 'sha256'
import store from '../store/index.js'
export default {
  methods: {
      async login(username, password){
          var hashPass = sha256(password);
            var _this = this;
          var Login = await fetch("http://localhost:300/api/users/login", {
              method: "POST",
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({"username": username, "password": hashPass})
          })
          .then(async function(response){
                if(response.status == 200){
                    _this.check = true;
                }
                return response.json();
            })
            .then(async function(data){
                if (_this.check == true){
                    store.commit('setToken', data.accessToken.token)
                    console.log(store.state.token)
                }else{
                    _this.check =  data.message;
                }
            })
      }
  },
  
  data () {
    return {
        username: "",
        password: ""
    }
  },
    
}
</script>

<style>
#container{
    border-color: #edebeb; /*#edebeb*/
    border-width: 5px;
    border-style: solid;
    border-radius: 20px;
}
#login{
    
    height: 400px;
    width: 400px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
</style>