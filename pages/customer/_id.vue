<template>
  <div  class="container">
    <div class="row my-4">
        <div class="col-md-6">
            <div class="customer-details">
                <h3 class="title bold">Customer Info</h3>
                <h5 class="name">Name: <span>{{customers.name}}</span></h5>
                <h5 class="name">Phone Number: <span>{{customers.phone_number}}</span></h5>
                <h5 class="village">Father Name: <span>{{customers.father_name}}</span></h5>
                <h5 class="village">Village: <span>{{customers.village}}</span></h5>
                <h5 class="village">Note: <span>{{customers.note}}</span></h5>
            </div>
        </div>
        <div class="col-md-6">
            <div class="row">
                <div class="col mb-2 col-6">
                     <div class="item">
                        <h4>Total Due</h4>
                        <h4 class="amount">{{customers.total_due}}tk</h4>
                    </div>
                </div>
                <div class="col mb-2 col-6">
                    <div class="item">
                        <h4>Last Payment</h4>
                        <h4 v-if="customers.transitions && customers.transitions.length" class="amount">{{customers.transitions[customers.transitions.length - 1].payment}}</h4>
                    </div>
                </div>
                <div class="col mb-2 col-6">
                    <div class="item">
                        <nuxt-link  v-if="customers._id" :to="'/edit-customer/'+customers._id" class="btn btn-success my-btn" type="submit">Edit Customer</nuxt-link>
                    </div>
                </div>
                
                <div class="col mb-2 col-6">
                     <div class="item">
                        <nuxt-link v-if="customers._id" :to="'/payment/persons?name='+customers._id" class="btn btn-success my-btn" type="submit">Payment</nuxt-link>
                    </div>
                </div>
                
            </div>
        </div>
    </div>
    <div class="transitions">
      <div class="row" v-if="customers.transitions && customers.transitions.length">
        <div class="list-group col-12 d-flex"   v-for="tr in customers.transitions.reverse()" :key="tr.id">
              <h5 class="number">{{tr.payment}}tk</h5>
              <h5 class="number">{{tr.createdAt}}</h5>
          </div>
      </div>
    </div>
  </div>

</template>

<script>
import Main from "~/components/home/main.vue"
import {API_URL} from "~/plugins/api"
export default {
  components: {Main},
  data(){
    return {
      customers: {},
    }
  },
  created() {
    this.getSingleCustomer()
  },
  methods: {
    getSingleCustomer(){
      if(process.browser && this.$route.query.name){
        let url = API_URL("customers/"+ this.$route.query.name+"/");
        let token = "Bearer " + localStorage.getItem("token")
        this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
            console.log(response)
            if(response.status == 200){
              this.customers = response.data;
            }
          }).catch(err => console.log(err));
      }
    }
  }
}
</script>

<style>
    .customer-details {
        padding: 30px;
        background: #fff;
        border-radius: 15px;
    }
    h3 {
        font-weight: bold;
        margin-bottom: 28px;
    }
    h5 span {
        font-weight: bold;
    }
    .col div {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background: #fff;
        padding: 30px;
        border-radius: 15px;
    }
    .amount {
        font-weight: bold;
    }
    .transitions .list-group {
      
    display: flex!important;
    align-items: center;
    justify-content: space-around;
    flex-direction: row;
    padding: 30px;
    background: #ffff;
    margin-bottom: 17px;
    border-radius: 15px;
    }
</style>
