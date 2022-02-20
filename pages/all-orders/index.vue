<template>
  <div  class="container">
    <h1>Orders</h1>
    <table v-if="orders.length > 0" class="table table-bordered table-dark">
      <thead>
        <tr>
          <th scope="col">Id</th>
          <th scope="col">Date</th>
          <th scope="col">Customer Name</th>
          <th scope="col">Order Total</th>
          <th scope="col">Customer Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in orders" :key="order.id">
          <th scope="row">{{order.id}}</th>
          <td v-if="order.created_at">{{order.created_at.split("T")[0]}}</td>
          <td v-if="order.customer">{{order.customer.name}}</td>
          <td>{{order.due}}</td>
          <td v-if="order.customer">{{order.customer.total_due}}</td>
          <td v-if="order.customer">{{order.customer.total_due}}</td>
        </tr>
      </tbody>
    </table>
  </div>

</template>

<script>
import {API_URL} from "~/plugins/api"
export default {
  data(){
    return {
      customers: [],
      orders: []
    }
  },
  created() {
    this.getOrder()
  },
  methods: {
    getOrder(){
      if(process.browser){
          let url = API_URL("orders/");
          let token = "Bearer " + localStorage.getItem("token")
          this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
            if(response.status == 200){
              this.orders = response.data.reverse();
              console.log(this.orders)
            }
          }).catch(err => console.log(err));
      }
    },
  }
}
</script>
