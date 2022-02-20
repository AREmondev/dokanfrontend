<template>
  <div  class="container">
    <Main v-if="customers && customers.length > 0" :customers="customers"/>
    <!-- <loader v-if="isLoading"/> -->
  </div>

</template>

<script>
import Main from "~/components/home/main.vue"
import Loader from "~/components/loader/loader.vue"
import {API_URL} from "~/plugins/api"
export default {
  components: {Main, Loader},
  data(){
    return {
      customers: [],
      isLoading: true
    }
  },
  created() {
    this.getUser()
  },
  methods: {
    getUser(){
      if(process.browser){
          let url = API_URL("customers?_limit=-1");
           let token = "Bearer " + localStorage.getItem("token")
           console.log(token)
          this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
            console.log(response)
            if(response.status == 200){
              this.isLoading = false;
              this.customers = response.data;
            }
          }).catch(err => console.log(err));
      }
    }
  }
}
</script>

<style>

</style>
