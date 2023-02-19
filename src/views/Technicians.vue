<script setup>
   import Navbar from '../components/Navbar.vue';
   import { ref , onMounted} from 'vue';
   import axios from 'axios';
import { BASE_URL } from '../commons';


   const isAdding = ref( false );
   const isLoading = ref( false );
   const name = ref('');
   const phone = ref('');
   const technicians = ref([]);//list of technicians
   const tryAddTechnician = async () => {
        isAdding.value = true;
        try{
            let result = await axios.post(`${BASE_URL}/technician`, {
                name : name.value,
                phone : phone.value
            });
            if( !result || !result.data ) return;
            technicians.value = [result.data,...technicians.value]

            //clear the forms
            name.value = '';
            phone.value = '';
        }catch( ex ) {
            //  console.log( ex );
        }finally {
            isAdding.value = false;
        }
   }

   const loadTechnicians = async () => {
        isLoading.value = true;
        try{
             let result = await axios.get(`${BASE_URL}/technician`);//retrieving all technicians
             if( !result || !result.data ) {technicians.value = []; return;}
             technicians.value = result.data
        }catch( ex ) {

        }finally {
            isLoading.value = false;
        }
   }

   onMounted( loadTechnicians );//method to be called on create
</script>

<template>
    <Navbar />
    <div class="container-fluid bg-danger">
       <div class="row">
           <div class="col-sm-12">
            <h2 class="text-light">Technicians Panel</h2>
           </div>
       </div>
     </div>
    <div class="container mt-4">
   
  <div class="row justify-content-between">
    <div class="col-6">
      <h4>Technicians list</h4>
        <table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">Name</th>
      <th scope="col">Phone</th>
      <th scope="col">Availability</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(tech, idx) in technicians" :key = "tech.id">
      <th scope="row">{{ idx+1 }}</th>
      <td>{{ tech.name }}</td>
      <td>{{ tech.phone }}</td>
      <td>xxx</td>
    </tr>
     
  </tbody>
</table>
    </div>
    <div class="col-6">
        <form class="row g-3">
            <h4>New technician</h4>
  <div class="col-md-6">
    <label for="techname" class="form-label">Technician Name</label>
    <input type="text" 
           v-model = "name"
           class="form-control" id="techname">
  </div>
  <div class="col-md-6">
    <label for="phone" class="form-label">Technician Phone</label>
    <input type="tel" class="form-control" 
       id="phone"
       v-model="phone"
       pattern="[0-9]{3}-[0-9]{3}-[0-9]{3}"
       required
    >
  </div>
  
  <div class="col-12">
    <button type="submit" class="btn btn-primary" :disabled = "isAdding || !phone.length || !name.length" @click="tryAddTechnician">
        <span v-if = "isAdding">Saving...</span> <span v-if = "!isAdding">Save</span>
    </button>
  </div>
</form>
    </div>
  </div>
</div>
</template>
