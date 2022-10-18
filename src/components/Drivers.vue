<script setup lang="ts">
import { onMounted, reactive } from "vue";
import axios from "axios";
import Loader from "../components/Loader.vue";

const restApiUri = "http://localhost:8000/api";

const data: any = reactive({
  drivers: new Array(),
  isLoading: true,
});

async function getAllDrivers() {
  const response = await axios.get(`${restApiUri}/drivers`);
  data.drivers = response.data;
  data.isLoading = false;
}

onMounted(() => {
  getAllDrivers();
});
</script>

<template>
  <div v-if="data.isLoading" class="loader">
    <Loader />
  </div>
  <div v-else="!data.isLoading" class="content">
    <div class="section-container">
      <h1>All drivers</h1>
      <div class="content-wrapper">
        <div class="driver-row-header">
          <p>ID</p>
          <p>NAME</p>
          <p>LASTNANAME</p>
          <p>DNI</p>
          <p>PHONE</p>
        </div>
        <div v-for="driver in data.drivers" class="driver-row">
          <p>{{ driver.id }}</p>
          <p>{{ driver.name }}</p>
          <p>{{ driver.lastname }}</p>
          <p>{{ driver.dni }}</p>
          <p>{{ driver.phone }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
