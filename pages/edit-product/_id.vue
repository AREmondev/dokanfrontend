<template>
  <div  class="container  py-5 ">
    <h1>Add Customer</h1>
    <form class="w-100  my-5">
        <div class="form-row row">
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Name</label>
            <input v-model="name"  type="text" class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="price">Price</label>
            <input v-model="price" id="price"  type="number" class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="stock">Stock</label>
            <input v-model="stock" id="stock"  type="number" class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Expire</label>
            <input v-model="expired"  type="date"  class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Expire</label>
            <input @change="uploadImg" id="input-file" type="file"  class="form-control" >
        </div>
        <button @click.prevent="makeProduct" class="btn btn-primary w-75 text-center my-2 mx-auto">Submit</button>
    </div>
</form>
  </div>

</template>

<script>
import Main from "~/components/home/main.vue"
import {API_URL} from "~/plugins/api"
export default {
  components: {Main},
  data(){
    return {
      name: '',
      price: '',
      stock: '',
      expired: '',
      productImg: '',
      productImgUrl: ''
    }
  },
  created() {
  },
  methods: {
    getSelectedProduct(){
          if(process.browser){
            let url = API_URL('products/'+ this.$route.params.id);
            let token = "Bearer " + localStorage.getItem("token");
            this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
                console.log(response)
                if(response.status == 200){
                    // this.name = response.data.name
                    // this.father_name= response.data.father_name ? response.data.father_name : ''
                    // this.phone= response.data.phone_number
                    // this.village= response.data.village
                    // this.due= response.data.total_due
                    // this.new= response.data.new
                }
            }).catch(err => console.log(err));
        }
      },
    uploadImg(){
      console.log(document.getElementById("input-file").files)
          if(document.getElementById("input-file")){
              if(document.getElementById("input-file").files[0]){
                  this.productImg = document.getElementById("input-file").files[0];
                  this.uploadImgInImgs()
              }
          }
      },
    makeProduct(){
      if(process.browser){
          let url = API_URL("products");
          let token = "Bearer " + localStorage.getItem("token");
          let data = {
              'name': this.name,
              'price': parseInt(this.price),
              'stock': parseInt(this.stock),
              'expired': this.expired,
              'imgUrl': this.productImgUrl,
            }
              console.log(data)
          this.$axios.post(url, data, {headers: {Authorization: token}}).then(response => {
              console.log(response)
              if(response.status == 200){
                this.name = ''
                this.father_name= ''
                this.phone= ''
                this.due= ''
              }
          }).catch(err => console.log(err));
      }
    },
    uploadImgInImgs(){
      if(process.browser){
          let url = API_URL("products");
          let token = "Bearer " + localStorage.getItem("token");

          const imgData = new FormData()
          imgData.set('key', 'f7f7f557e9e948e808c9ac2bf760a395')
          imgData.append('image', this.productImg)

          this.$axios.post("https://api.imgbb.com/1/upload", imgData).then(response => {
              if(response.status == 200){
                this.productImgUrl = response.data.data.url
              }
          }).catch(err => console.log(err));
      }
    },
  }
}
</script>

<style>
  .form-group {
    margin-bottom: 18px;
  }
</style>
