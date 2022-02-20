<template>
    <div>
        <nav class="navbar navbar-expand-lg  navbar-light bg-light">
            <div class="container">
            <nuxt-link to="/"  class="navbar-brand" href="#">Syngenta Shop</nuxt-link>

            <div class="collapse navbar-collapse justify-content-end" id="navbarTogglerDemo03">
                <div v-if="user.username != ''" class="d-flex align-items-conter">
                    <nuxt-link class="btn btn-success m-2" to="/add-customer">Add Customer</nuxt-link>
                    <nuxt-link class="btn btn-success m-2" to="/all-product">Product</nuxt-link>
                    <nuxt-link class="btn btn-success m-2" to="/add-product">Add Product</nuxt-link>
                    <nuxt-link class="btn btn-success m-2" to="/overview">Overview</nuxt-link>
                </div>
                <form v-else class="form-inline d-flex align-items-center my-2 ml-0 my-lg-0">
                    <input v-model="userName" class="form-control m-2" type="email" placeholder="Email">
                    <input v-model="password" class="form-control m-2" type="password" placeholder="Password">
                    <button @click.prevent="login"  class="btn btn-outline-success my-2 my-sm-0">Login</button>
                </form>

            </div>
            </div>    
        </nav>
    </div>
</template>

<script>
import {API_URL} from "~/plugins/api"

export default {
    data(){
        return {
            isLogin: false,
            userName: '',
            password: '',
            user: {
                'username': '',
                'email': ''
            }
        }
    },
    created() {
        this.checkAuth()
    },
    methods: {
        login(){
            let url = API_URL("auth/local/");
            let data = {
                "identifier": this.userName,
                "password": this.password
            }
            console.log(data)
            this.$axios.post(url, data).then(response => {
                if(response.status == 200){
                    let userJWT = response.data.jwt
                    let userName = response.data.user.username
                    localStorage.setItem("token", userJWT)
                    localStorage.setItem("userName", JSON.stringify(userName))
                    this.checkAuth()
                }
            }).catch(err => console.log(err));
        },
        checkAuth(){
            if(process.browser){
                let token = localStorage.getItem("token")
                let userName = localStorage.getItem("userName")
                console.log(userName)
                if(token){
                    this.user.username = JSON.parse(userName)
                    this.isLogin = true
                }else {
                    this.isLogin = false
                }
            }
        }
    }
}
</script>