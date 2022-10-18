<script lang="ts" setup>
import axios from "axios";
import { onMounted, reactive } from "vue";
import Loader from "../components/Loader.vue";

const restApiUri = "http://localhost:8000/api";

const data: any = reactive({
  vehicles: new Array(),
  isLoading: true,
});

async function getAllVehicles() {
  const response = await axios.get(`${restApiUri}/vehicles`);
  data.vehicles = response.data;
  data.isLoading = false;
}

onMounted(() => {
  getAllVehicles();
});
</script>

<template>
  <div v-if="data.isLoading" class="loader">
    <Loader />
  </div>
  <div v-else="!data.isLoading" class="content">
    <div class="section-container">
      <h1>All vehicles</h1>
      <div class="content-wrapper">
        <div class="vehicle-row-header">
          <p>ID</p>
          <p>PLATENUMBER</p>
          <p>BRAND</p>
          <p>MODEL</p>
          <p>YEAR</p>
        </div>
        <div v-for="vehicle in data.vehicles" class="vehicle-row">
          <p>{{ vehicle.id }}</p>
          <p>{{ vehicle.platenumber }}</p>
          <p>{{ vehicle.brand }}</p>
          <p>{{ vehicle.model }}</p>
          <p>{{ vehicle.year }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
