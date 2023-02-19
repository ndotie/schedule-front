<script setup>
import { ref,onMounted } from 'vue';
import { BASE_URL } from '../commons';

  const props = defineProps(["tech","show","close"])
  const close = props.close
 
</script>
<template>
    <div  class="cu_modal" :class="showModel">
    <!-- Modal content -->
    <div class="cu_modal-content">
    <span class="cu_close" @click ="close">&times;</span>
    <h4>Assign Technician</h4>
   
    <slot></slot>
    </div>

    </div>
</template>

<style scoped>
    /* The Modal (background) */

.cu_modal {
  /* Hidden by default */
  display: block;
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.cu_modal-content {
  background-color: #fefefe;
  margin: 10% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
  max-width: 500px;
  border-radius: 10px;
}

/* The Close Button */
.cu_close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.cu_close:hover,
.cu_close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>