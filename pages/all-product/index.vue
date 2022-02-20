<template>
  <div  class="container">
    <h1 class="py-5">Products</h1>
    <div class="row">
        <div class="col-md-4 mb-3" v-for="pd in products" :key="pd.id">
            <div class="card" >
                <img class="card-img-top" :src="pd.imgUrl" alt="Card image cap p-4">
                <div class="card-body">
                    <h5 class="card-title">{{pd.name}}</h5>
                    <h6 class="card-text">Expire with in {{
    Math.round((new Date(pd.expired).getTime() - new Date().getTime() ) / (1000 * 3600 * 24))
                        }} days</h6>
                    <a href="#" class="btn btn-primary">Stock {{pd.stock}}</a>
                </div>
            </div>
        </div>
    </div>
  </div>

</template>

<script>
import {API_URL} from "~/plugins/api"
export default {
  data(){
    return {
      customers: [],
      orders: [],
      products:[]
    }
  },
  created() {
    this.getOrder()
  },
  methods: {
    getOrder(){
      if(process.browser){
          let url = API_URL("products/");
          let token = "Bearer " + localStorage.getItem("token")
          this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
            console.log(response)
            if(response.status == 200){
              this.products = response.data
            //   console.log(this.orders)
            }
          }).catch(err => console.log(err));
      }
    },
  }
}
</script>
