<template>
    <client-only>
        <div class="py-5">
            <Search/>
            <div class="input-group mb-3">
                <select @change="filterByVillage" v-model="village" id="village" class="form-select" aria-label="Default select example">
                   <option value="Andhorail">Andhorail</option>
              <option value="Dorbeshpur">Dorbeshpur</option>
              <option value="dulahar">dulahar</option>
              <option value="Nasirabad">Nasirabad</option>
              <option value="Murshida">Murshida</option>
              <option value="Mirzapur">Mirzapur</option>
              <option value="Mirapara">Mirapara</option>
              <option value="Amjoan">Amjoan</option>
              <option value="Pirpur">Pirpur</option>
              <option value="Lokkhonpur">Lokkhonpur</option>
              <option value="Nachole">Nachole</option>
              <option value="Muradpur">Muradpur</option>
              <option value="Maktapur">Maktapur</option>
              <option value="Konnanogor">Konnanogor</option>
              <option value="Surjopur">Surjopur</option>
              <option value="Verendi">Verendi</option>
              <option value="Rahoigram">Rahoigram</option>
              <option value="Sonabor">Sonabor</option>
              <option value="Surjopur">Surjopur</option>
              <option value="Muradpur">Muradpur</option>
              <option value="others">others</option>
                </select>
                <input @keyup="search" v-model="search_text" type="search" class="form-control p-2" aria-label="Text input with dropdown button">
                <button class="btn btn-outline-secondary" type="button" id="inputGroupFileAddon04">Search</button>
            </div>
            <div class="list-group" v-if="filterData.length > 0">
                <div class="list-group-item row" v-for="customer in filterData" :key="customer.id">
                    <!-- <img src="https://images.rawpixel.com/image_png_800/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvdjc5MS10YW5nLTM1LnBuZw.png?s=aLxshBxLcykO2UAnr6F0Nzhqtdx6iR6UuKi4bFSTzC8" alt=""> -->
                    <div class="col">
                        <nuxt-link class="name" :to="'/customer/persons?name='+customer._id">{{customer.name}}</nuxt-link>
                    </div>
                    <div class="col">
                       <h5>{{customer.village}}</h5>
                    </div>
                    <div class="col">
                        <h5 class="number">{{customer.total_due}}tk</h5>
                    </div>
                    <div class="col">
                        <h5 class="number">{{customer.phone_number}}</h5>
                    </div>
                    <div class="col">
                        <nuxt-link :to="'/order/persons?name='+customer.id" class="number">Order</nuxt-link>
                    </div>
                    <div class="col">
                        <nuxt-link :to="'/payment/persons?name='+customer.id" class="number">Payment</nuxt-link>
                    </div>
                </div>
            </div>
        </div>
    </client-only>
</template>

<script>
import Search from "~/components/search/search.vue"
export default {
    components: {Search},
    props: {
        customers: {
            type: Array,
            default: [],
            required: true,
        }
    },
    data(){
        return {
             search_text: '',
             filterData: this.$props.customers,
             village: ''
        }
    },
    created() {
    },
    methods:{
        search(){
            this.filterData = this.filterData.filter(data => data.name.toLowerCase().includes(this.search_text.toLowerCase()))
        },
        filterByVillage(){
            this.filterData = []
            if(this.$props.customers){
                for(let data of this.$props.customers){
                    if(this.village.toLowerCase() == "all"){
                        this.filterData = this.$props.customers
                    }
                    if(this.village.toLowerCase() == data.village.toLowerCase()){
                        this.filterData.push(data)
                    }
                }
            }
        }
    }
}
</script>

<style>
.list-group-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-bottom: 0.7rem;
    border: none;
    background-color: #ffffff;
    box-shadow: 0 0 15px #fff;
    border-radius: 0.3rem;
}
li.list-group-item img {
    height: 60px !important;
    min-width: 60px !important;
    width: 60px !important;
    border-radius: 60px;
}
a.name {
    font-size: 1.8rem;
    color: rgb(15, 15, 15);
    text-decoration: none;
}
</style>