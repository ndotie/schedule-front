<script setup>
    import { ref , watch, onMounted } from 'vue';
    import axios from 'axios';
    import moment from 'moment'
import { BASE_URL } from '../commons';
import ScheduleModel from './ScheduleModel.vue';
    const props = defineProps(["technician", "day"]) //we need to add day date and check when changes
    const isLoading = ref( false );
    const showModel = ref( false );
    const schedules = ref([])
    const loadTimeTable = async () => {
        isLoading.value = true;
        try{
          let result = await axios.get(`${BASE_URL}/techschedule/${moment(props.day).format('YYYY-MM-DD')}/${props.technician.id}`)
          console.log( result );
          if( !result || !result.data ) {
            schedules.value = [];
            return;
          }
          schedules.value = result.data;

        }catch( ex ){
          schedules.value = [];
        }finally{
            isLoading.value = false;
        }
    }
    onMounted( loadTimeTable );
    watch( () => props.day,  async () => { 
      await loadTimeTable();
    })
    const toggleModel = () => { showModel.value = !showModel.value }

  const ordersList = ref([])
  const isLoadingOrders = ref( false );
  const loadOrders = async () => {
        isLoadingOrders.value = true;
        try{
             let result = await axios.get(`${BASE_URL}/orders`);//retrieving all orders
             if( !result || !result.data ) {ordersList.value = []; return;}
             ordersList.value = result.data
        }catch( ex ) {
            ordersList.value = []
        }finally {
            isLoadingOrders.value = false;
        }
   }

   watch(showModel, async( newShowStatus ) => {
      if( !newShowStatus ) return;
      await loadOrders()
   })

   const isAssigning = ref( false );
   const order_id = ref('');
   //we already have technician id
   const start_at = ref('');
   const end_at = ref('');
   const day = ref('');
   const assignTechnician = async () => {
       isAssigning.value = false;
       try {
          let results = await axios.post( `${BASE_URL}/schedules`, {
             day : day.value,
             end_at : end_at.value+":00", //:00 to comply with time standard
             start_at : start_at.value+":00",
             order_id : order_id.value,
             technician_id : props.technician.id
          } );
       } catch ( ex ) {

       } finally {
        isAssigning.value = false;
        await loadTimeTable();
       }
   }
   
</script>
<!-- style="max-width: 18rem;" -->
<template>
    <div class="card border-dark mb-3" >
  <div class="card-header justify-content-between d-flex">
        <span>{{ props.technician.name }} #{{ schedules.length }}</span>
        <i class="bi bi-pencil text-info" @click="toggleModel"></i>
  </div>
  <div class="card-body text-dark p-0">
     <!-- start of the list -->
  <ol class="list-group list-group-numbered">
    <li class="list-group-item d-flex justify-content-between align-items-start"
        v-for="(sche,idx) in schedules" :key="sche.id">
      <div class="ms-2 me-auto">
        <div class="fw-bold">{{ sche.title }} #{{ sche.refr }}</div>
        
        <small><i class="bi bi-clock-fill text-success text-info"></i> {{ sche.start_at }} - {{ sche.end_at }}</small>
      </div>
      <!-- <i class="bi bi-clock-fill text-success text-info"></i> -->
    </li>
  </ol>



     <!-- end of the list -->
  <ScheduleModel  :tech = "props.technician" 
                    :close = "toggleModel"
                    v-show="showModel">
            
                    <form class="row g-3">
                      
  <div class="col-md-12">
    <label for="inputState" class="form-label">Select Order</label>
    <select id="inputState" class="form-select" v-model="order_id">
      <option disabled  value = "">Choose...</option>
      <option v-for="(order, idx) in ordersList" 
               :key = "order.id" :value = "order.id">{{ order.refr }} - {{ order.title }}</option>
    </select>
  </div> 
  <div class="col-md-4">
    <label for="date" class="form-label">Date</label>
    <input type="date" 
           class="form-control" 
           v-model="day"
           id="date">
  </div>
  <div class="col-md-4">
    <label for="start_at" class="form-label">Start</label>
    <input type="time" 
           class="form-control" 
           v-model = "start_at"
           id="start_at">
  </div>
  <div class="col-md-4">
    <label for="end_at" class="form-label">End</label>
    <input type="time" 
           class="form-control" 
           v-model="end_at"
           id="end_at">
  </div> 
  
  <div class="col-12">
    <button type="submit" class="btn btn-primary" @click.prevent="assignTechnician">
      <span v-if = "isAssigning">Saving...</span> <span v-if = "!isAssigning">Save</span>
    </button>
  </div>
</form>
    </ScheduleModel>
  </div>
</div>

</template>