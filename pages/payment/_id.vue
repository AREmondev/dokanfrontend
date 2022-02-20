<template>
    <client-only>
        <div class="container py-5">
            <form class="w-100 mb-5">
                 <div class="form-row row">
                    <div class="form-group col-md-4">
                        <label for="inputEmail4">Name</label>
                        <input v-model="ordererUser.name" disabled type="text" class="form-control" >
                    </div>
                    <div class="form-group col-md-4">
                        <label for="inputEmail4">Village</label>
                        <input v-model="ordererUser.village" disabled type="text" class="form-control" >
                    </div>
                    <div class="form-group col-md-2">
                        <label for="inputEmail4">Due</label>
                        <input v-model="ordererUser.total_due" disabled type="text"  class="form-control" >
                    </div>
                    <div class="form-group col-md-2 d-flex align-items-center justify-content-between">
                        <nuxt-link v-if="ordererUser._id" :to="'/customer/persons?name='+ordererUser._id" class="btn btn-success my-btn" type="submit">Customer Details</nuxt-link>
                    </div>
                </div>
            </form>

            <form @submit.prevent="applyPayment">
                <div class="form-group col">
                    <label for="payment">Payment</label>
                    <input id="payment" v-model="payment" type="number"  class="form-control" >
                </div>
                <button class="btn btn-success" type="submit">Payment</button>
            </form>
            
        </div>
    </client-only>
</template>

<script>
import {API_URL} from "~/plugins/api"
import Order from "~/components/order/order.vue"
export default {
    components: {Order},
    data(){
        return {
        ordererUser: {},
        products: [],
        payment: ''
        }
    },
    created() {
        this.getOrdererUser();
    },
    cumputed: {
        
    },
    methods: {
        applyPayment(){
             if(process.browser){
                var today = new Date()
                let url = API_URL(`transitions/`);
                let data = {
                    "time": today,
                    "payment": this.payment,
                    "customer_id": this.$route.query.name
                }
                console.log(data)
                let token = "Bearer " + localStorage.getItem("token")
                this.$axios.post(url, data, {headers: {Authorization: token}}).then(response => {
                    console.log(response)
                    if(response.status == 200){
                        this.ordererUser = response.data;
                        this.payment = ''
                    }
                }).catch(err => console.log(err));
            }
        },
        getOrdererUser(){
            if(process.browser && this.$route.query.name){
                let url = API_URL(`customers/${this.$route.query.name}`);
                let token = "Bearer " + localStorage.getItem("token")
                this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
                    if(response.status == 200){
                        this.ordererUser = response.data;
                    }
                }).catch(err => console.log(err));
            }
        },
    }
}
</script>

<style>
.my-btn {
    margin-bottom: -20px;
}
</style>