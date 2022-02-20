<template>
    <client-only>
        <div class="container py-5">
            <form class="w-100 mb-5">
                 <div class="form-row row">
                    <div class="form-group col-md-5">
                        <label for="inputEmail4">Name</label>
                        <input v-model="ordererUser.name" disabled type="text" class="form-control" >
                    </div>
                    <div class="form-group col-md-5">
                        <label for="inputEmail4">Village</label>
                        <input v-model="ordererUser.village" disabled type="text" class="form-control" >
                    </div>
                    <div class="form-group col-md-2">
                        <label for="inputEmail4">Due</label>
                        <input v-model="ordererUser.total_due" disabled type="text"  class="form-control" >
                    </div>
                </div>
            </form>
            <Order :userId="userId" :products="products" />
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
        userId: this.$route.query.name
        }
    },
    created() {
        this.getOrdererUser();
        this.getProducts();
    },
    cumputed: {
        
    },
    methods: {
        getOrdererUser(){
            if(process.browser){
                let url = API_URL(`customers/${this.$route.query.name}`);
                let token = "Bearer " + localStorage.getItem("token")
                this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
                    if(response.status == 200){
                    this.ordererUser = response.data;
                    }
                }).catch(err => console.log(err));
            }
        },
        getProducts(){
            if(process.browser){
                let url = API_URL("products/");
                let token = "Bearer " + localStorage.getItem("token")
                this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
                    if(response.status == 200){
                        this.products = response.data;
                    }
                }).catch(err => console.log(err));
            }
        }
    }
}
</script>

<style>

</style>