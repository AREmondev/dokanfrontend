<template>
    <client-only>
        <div class="container py-5">
            <Order :userId="userId" :products="products" />
        </div>
    </client-only>
</template>

<script>
import {API_URL} from "~/plugins/api"
import Order from "~/components/stock/stock.vue"
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