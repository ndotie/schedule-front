<script setup>
   import Navbar from '../components/Navbar.vue';
   import { ref , onMounted} from 'vue';
   import axios from 'axios';
import { BASE_URL } from '../commons';


   const isAdding = ref( false );
   const isLoading = ref( false );
   const title = ref('');
   const refr = ref('');

   const ordersList = ref([]);//list of orders
   const tryAddOrders = async () => {
        isAdding.value = true;
        try{
            let result = await axios.post(`${BASE_URL}/orders`, {
                title : title.value,
                refr : refr.value,
                status : true
            });
            if( !result || !result.data ) return;
            ordersList.value = [result.data,...ordersList.value]

            //clear the forms
            title.value = '';
            refr.value = '';
        }catch( ex ) {
            //  console.log( ex );
        }finally {
            isAdding.value = false;
        }
   }

   const loadOrders = async () => {
        isLoading.value = true;
        try{
             let result = await axios.get(`${BASE_URL}/orders`);//retrieving all orders
             if( !result || !result.data ) {ordersList.value = []; return;}
             ordersList.value = result.data
        }catch( ex ) {

        }finally {
            isLoading.value = false;
        }
   }

   onMounted( loadOrders );//method to be called on create
</script>

<template>
    <Navbar />
     <div class="container-fluid bg-danger">
       <div class="row">
           <div class="col-sm-12">
            <h2 class="text-light">Orders Panel</h2>
           </div>
       </div>
     </div>
    <div class="container mt-4">
   
  <div class="row justify-content-between">
    <div class="col-6">
      <h4>Orders List</h4>
        <table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">ID</th>
      <th scope="col">Title</th>
      <th scope="col">Status</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(order, idx) in ordersList" :key = "order.id">
      <th scope="row">{{ idx+1 }}</th>
      <td>{{ order.refr }}</td>
      <td>{{ order.title }}</td>

      <td>xx</td>
    </tr>
     
  </tbody>
</table>
    </div>
    <div class="col-6">
      <h4>Add order</h4>
        <form class="row g-3" >
            
  <div class="col-md-6">
    <label for="techname" class="form-label">Title</label>
    <input type="text" 
           v-model = "title"
           class="form-control" id="techname">
  </div>
  <div class="col-md-6">
    <label for="phone" class="form-label">Order Number</label>
    <input type="number" class="form-control" 
       id="phone"
       v-model="refr"
       required
    >
  </div>
  
  <div class="col-12">
    <button type="submit" class="btn btn-primary" 
            :disabled = "isAdding || !refr || !title.length" @click.prevent="tryAddOrders">
        <span v-if = "isAdding">Saving...</span> <span v-if = "!isAdding">Save</span>
    </button>
  </div>
</form>
    </div>
  </div>
</div>
</template>
