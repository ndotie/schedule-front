<script setup> 
// import FullCalendar from '@fullcalendar/vue3'
// import dayGridPlugin from '@fullcalendar/daygrid'
import { BASE_URL } from '../commons';

import Navbar from '../components/Navbar.vue';
import TechnicianOrders from '../components/TechnicianOrders.vue';
import { ref, onMounted }  from 'vue';
import axios from 'axios';
import moment from 'moment';
const isLoading = ref( false );
const technicians = ref( [] );
const date = ref(moment());
const counter = ref(0);
const nextday = () => {
     counter.value = counter.value + 1
    date.value = moment( date ).add(counter.value,'day')
}
const prevday = () => {
   
  counter.value = counter.value - 1
    date.value = moment( date ).subtract(counter.value,'day')
}
const loadTechnicians = async () => {
      isLoading.value = true;
      try{
            let result = await axios.get(`${BASE_URL}/technician`);//retrieving all technicians
            if( !result || !result.data ) {technicians.value = []; return;}
            technicians.value = result.data
      }catch( ex ) {
        // console.log( ex )
      }finally {
          isLoading.value = false;
      }
  }

  onMounted( loadTechnicians );//method to be called on create
</script>

<template>
  <Navbar />
  <div class="container-fluid mb-3 bg-danger">
    <div class="container">
    <div class="row justify-content-between ">
      <div class="col-sm-4">
        <h3 class="text-light">Schedule panel</h3>
      </div>
        
      <div class="col-sm-3">
        <div class="d-flex justify-content-around align-items-center">
          <i class="bi bi-arrow-left-circle-fill" 
             style="font-size: 2rem; color: cornflowerblue;cursor: pointer;"
             @click = "prevday"></i>
            <span class="text-light"> {{ moment(date).format('MMM Do, YY') }} </span>
          <i class="bi bi-arrow-right-circle-fill" 
             style="font-size: 2rem; color: cornflowerblue;cursor: pointer;"
             @click="nextday"></i>
        </div>
      </div>
    </div>
   </div> 
  </div>
    <div class="container">
    <div class="row justify-content-start">
      <div class="col-sm-3" 
           v-for="(tech,idx) in technicians" :key = "tech.id"
      >
      <TechnicianOrders :technician = "tech" :day = "date"/>
    </div>
    </div>
    
  </div>
</template>
