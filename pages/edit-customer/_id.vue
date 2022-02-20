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
            <label for="f_name">Father Name</label>
            <input v-model="father_name" id="f_name"  type="text" class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Phone</label>
            <input v-model="phone"  type="text"  class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Due</label>
            <input v-model="due"  type="text"  class="form-control" >
        </div>
        
        
        <button @click.prevent="updateCustomer" class="btn btn-primary w-75 text-center my-2 mx-auto">Submit</button>
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
      phone: '',
      name: '',
      due: '',
      village: '',
      new_session: false,
      father_name: ''
    }
  },
  created() {
      this.getSelectedCustomer();
  },
  methods: {
      getSelectedCustomer(){
          if(process.browser){
            let url = API_URL('customers/'+ this.$route.params.id);
            let token = "Bearer " + localStorage.getItem("token");
            this.$axios.get(url, {headers: {Authorization: token}}).then(response => {
                console.log(response)
                if(response.status == 200){
                    this.name = response.data.name
                    this.father_name= response.data.father_name ? response.data.father_name : ''
                    this.phone= response.data.phone_number
                    this.village= response.data.village
                    this.due= response.data.total_due
                    this.new= response.data.new
                }
            }).catch(err => console.log(err));
        }
      },
    updateCustomer(){
      if(process.browser){
          let url = API_URL("customers/"+ this.$route.params.id);
          let token = "Bearer " + localStorage.getItem("token");
           let data = {
              'name': this.name,
              'Father_name': this.father_name,
              'phone_number': this.phone,
              'village': this.village,
              'total_due': parseInt(this.due),
              'previous_due':  parseInt(this.due),
              'new':  this.new_session,
              'note':  this.note,
              }
          this.$axios.put(url, data, {headers: {Authorization: token}}).then(response => {
              if(response.status == 200){
                  alert("Customer Updated")
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
