<template>
    <div>
        <v-app>
            <p>TEST</p>
            <v-btn @click="test()">TEST</v-btn>
        </v-app>
    </div>
</template>

<script>
import store from '../store/index.js'
export default {
  created () {
      console.log(store.state.token)
  },
  data () {
    return {
    }
  },
  methods: {
      async test(){
        var _this = this;
        await fetch("http://localhost:300/api/test", {
              method: "GET",
              headers: {'Authorization': 'Bearer ' + store.state.token}
          })
          .then(async function(response){
                if(response.status == 200){
                    _this.check = true;
                }
                return response.json();
            })
            .then(async function(data){
                if (_this.check == true){
                    console.log(data)
                }else{
                    _this.check =  data.message;
                }
            })
      }
  },
    
}
</script>
