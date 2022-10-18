<script setup lang="ts">
import { onMounted, reactive, watchEffect } from "vue";
import axios from "axios";
import Loader from "./Loader.vue";
const restApiUri = "http://localhost:8000/api";

const props = defineProps(["onSuccess", "onCancel"]);

const data = reactive({
  availableDrivers: new Array(),
  availableVehicles: new Array(),
  isLoading: true,
  selectedDriver: "-1",
  selectedVehicle: "-1",
});

async function getAvailableDrivers() {
  const response = await axios.get(`${restApiUri}/drivers/available`);
  data.availableDrivers = response.data;
}

async function getAvailableVehicles() {
  const response = await axios.get(`${restApiUri}/vehicles/available`);
  data.availableVehicles = response.data;
}

function init() {
  Promise.all([getAvailableDrivers(), getAvailableVehicles()]).then(() => {
    data.isLoading = false;
  });
}

function handleOnVehicleChange(e: any) {
  data.selectedVehicle = e.target.value;
}

function handleOnDriverChange(e: any) {
  data.selectedDriver = e.target.value;
}

async function handleOnConfirm() {
  const res = await axios.post(`${restApiUri}/assign`, {
    vehicle: parseInt(data.selectedVehicle),
    driver: parseInt(data.selectedDriver),
  });

  console.log(res.data);

  props.onSuccess();
}
onMounted(() => {
  init();
});
</script>

<template>
  <div class="modal">
    <div class="modal-container">
      <div v-if="data.isLoading" class="modal-loader">
        <Loader />
      </div>
      <div v-else="!data.isLoading" class="modal-content">
        <div style="display: flex; justify-content: flex-end">
          <button class="cancel-button" @click="props.onCancel">X</button>
        </div>
        <h1>New Assignment</h1>
        <p>
          Select a vehicle and a driver. Only available vehicles and drivers are
          displayed.
        </p>
        <div>
          <select @change="handleOnVehicleChange">
            <option label="Select a vehicle" selected value="-1" />
            <option
              v-for="vehicle in data.availableVehicles"
              v-bind:value="vehicle.id"
              v-bind:label="
                '[' +
                vehicle.id +
                ']' +
                ' - ' +
                vehicle.platenumber +
                ' - ' +
                vehicle.brand +
                ' - ' +
                vehicle.model
              "
            />
          </select>
        </div>
        <div>
          <select @change="handleOnDriverChange">
            <option label="Select a driver" selected value="-1" />
            <option
              v-for="driver in data.availableDrivers"
              v-bind:value="driver.id"
              v-bind:label="
                '[' +
                driver.id +
                ']' +
                ' - ' +
                driver.name +
                ' - ' +
                driver.lastname +
                ' - ' +
                driver.dni +
                ' - ' +
                driver.phone
              "
            />
          </select>
        </div>
        <div class="btn-wrapper">
          <button
            class="btn-confirm"
            :disabled="
              data.selectedDriver === '-1' || data.selectedVehicle === '-1'
            "
            @click="handleOnConfirm"
          >
            CONFIRM
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
h1 {
  color: #fff;
}

.modal {
  position: fixed;
  z-index: 9999;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #000000cc;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-container {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  width: 80vw;
}

@media (min-width: 768px) {
  .modal-container {
    max-width: 500px;
  }
}

.modal-loader {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.modal-content select {
  width: 100%;
  padding: 5px 10px;
  margin: 10px 0;
  background-color: #fff;
  color: #263238;
  outline: none;
}

.modal-content p {
  color: #263238;
}

.btn-confirm {
  padding: 10px 20px;
  border: none;
  color: white;
  background-color: #f1c40f;
  color: #263238;
  font-weight: 600;
}

.btn-confirm:hover {
  background-color: #f7dc6f;
  cursor: pointer;
}

.btn-confirm:disabled {
  background-color: #546e7a;
}

.btn-confirm:disabled:hover {
  background-color: #546e7a;
  cursor: default;
}

.btn-wrapper {
  display: flex;
  flex-direction: flex;
  justify-content: flex-end;
  margin-top: 20px;
}

.cancel-button {
  background-color: #f44336;
  border: none;
  color: #fff;
  padding: 10px 15px;
  font-weight: 600;
}

.cancel-button:hover {
  background-color: #d32f2f;
  cursor: pointer;
}
</style>
