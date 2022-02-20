<template>
<client-only>
    <div  class="container">
    <div class="row my-4">
        <div class="col-md-4 mb-3">
            <div class="customer-details">
                <div class="totalPayout">
                     <h4 class="m-0 text-center">Previous Due: {{prev_due}}TK</h4>
                </div>
            </div>
        </div>
        <div class="col-md-4 mb-3">
            <div class="customer-details">
                <div class="totalPayout">
                     <h4 class="m-0 text-center">New Due: {{new_due}}TK</h4>
                </div>
            </div>
        </div>
        <div class="col-md-4 mb-3">
            <div class="customer-details">
                <div class="totalPayout">
                     <h4 class="m-0 text-center">Total Due: {{total_due}}TK</h4>
                </div>
            </div>
        </div>
        <div class="col-md-4 mb-3">
            <div class="customer-details">
                <div class="totalPayout">
                     <h4 class="m-0 text-center">Total Payment: {{total_payment}}TK</h4>
                </div>
            </div>
        </div>
        
    </div>
    <div class="transitions">
      <div class="row" v-if="payoutHistory && payoutHistory.length">
        <div class="list-group col-12 d-flex"   v-for="tr in payoutHistory.reverse()" :key="tr.id">
              <h5 class="number">{{tr.time}}tk</h5>
              <h5 class="number">{{tr.paymet}}</h5>
          </div>
      </div>
    </div>
  </div>

</client-only>
</template>

<script>
import Main from "~/components/home/main.vue"
import {API_URL} from "~/plugins/api"
export default {
  name: "TotalDue",
  data(){
    return {
      total_due: 0,
      new_due: 0,
      prev_due: 0,
      transitions: [],
      paymentByDate: [],
      total_payment: '',
      payoutHistory: []
    }
  },
  created() {
    this.getAllCustomersTotalDue();
    this.getAllCustomersTransition();
  },
  methods: {
    getAllCustomersTotalDue(){
      if(process.browser){
        let url = API_URL("customers?_limit=-1");
        let token = "Bearer " + localStorage.getItem("token")
        this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
            if(response.status == 200){
              let customers = response.data;
              var totalDue = 0
              var newDue = 0
              var prevDue = 0
              for(let due of customers){
                  totalDue += parseInt(due.total_due)
                  if(due.new){
                      newDue += parseInt(due.total_due)
                  }else {
                      prevDue += parseInt(due.total_due)
                  }
              }
              this.total_due = totalDue
              this.new_due = newDue
              this.prev_due = prevDue
              console.log(this.total_due)
            }
          }).catch(err => console.log(err));
      }
    },
    getAllCustomersTransition(){
      if(process.browser){
        let url = API_URL("transitions/");
        let token = "Bearer " + localStorage.getItem("token")
        this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
            if(response.status == 200){
                this.transitions = response.data
                let total_payment = 0
                let paymentByDate = []
                let history = []
                for(let payment of this.transitions){
                    if(!paymentByDate.includes(payment.time)){
                        paymentByDate.push(payment.time)
                    }
                    total_payment += parseInt(payment.payment)
                }
                this.paymentByDate = paymentByDate
                this.getDateTransition()
                this.total_payment = total_payment
            }
          }).catch(err => console.log(err));
      }
    },
    getDateTransition(){
        let payoutHistory = []
        for(let data of this.paymentByDate){
            let total_payment = 0
            for(let payment of this.transitions){
                if(payment.time == data){
                   total_payment += parseInt(payment.payment)
                }
            }
            payoutHistory.push({"paymet": total_payment, "time": data})

        }
        this.payoutHistory = payoutHistory
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
