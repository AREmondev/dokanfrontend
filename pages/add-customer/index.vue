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
            <label for="village">Village</label>
            <select v-model="village" id="village" class="form-select" aria-label="Default select example">
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
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Phone</label>
            <input v-model="phone"  type="text"  class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4">
            <label for="inputEmail4">Due</label>
            <input v-model="due"  type="text"  class="form-control" >
        </div>
        <div class="form-group col-md-6 col-xl-4 d-flex align-items-center">
          <label for="exampleFormControlTextarea1">Note</label>
          <textarea   v-model="note"  class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
        </div>
        <div class="form-group col-md-6 col-xl-4 d-flex align-items-center">
          <label class="m-2" for="new">New</label>
          <input id="new"  v-model="new_session"  class="form-check-input" type="checkbox" value="" aria-label="Checkbox for following text input">
        </div>
        
        <button @click.prevent="makeCustomer" class="btn btn-primary w-75 text-center my-2 mx-auto">Submit</button>
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
  },
  methods: {
    makeCustomer(){
      if(process.browser){
          let url = API_URL("customers?_limit=-1");
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
  }
}
</script>

<style>
  .form-group {
    margin-bottom: 18px;
  }
</style>
