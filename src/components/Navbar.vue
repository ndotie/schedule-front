<script setup>
    import { RouterLink } from 'vue-router';
</script>
<template>
    
    
    

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Schedulr</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
            <RouterLink to = "/" class="nav-link" active-class="active">Schedule</RouterLink>
        </li>
        <li class="nav-item">
            <RouterLink to="/orders" class="nav-link" active-class="active">Orders</RouterLink>
        </li> 
        <li class="nav-item">
            <RouterLink to = "/technicians" class="nav-link" active-class="active">Technicians</RouterLink>
        </li>
      </ul>
      <!-- <form class="d-flex">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form> -->
    </div>
  </div>
</nav>
</template>