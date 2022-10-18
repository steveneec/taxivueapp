<script setup lang="ts">
import { onMounted, reactive } from "vue";
import axios from "axios";
import Loader from "./Loader.vue";
import NewAssign from "./NewAssign.vue";

const restApiUri = "http://localhost:8000/api";

const data: any = reactive({
  assigns: new Array(),
  isLoading: true,
  isAddingNewAssignment: false,
});

async function getAssigns() {
  const response = await axios.get(`${restApiUri}/assigned`);
  data.assigns = response.data;
}

async function handleOnRemove({ id }: { id: number }) {
  try {
    const response = await axios.delete(`${restApiUri}/assign/remove/${id}`);
    console.log(response.data);
    //reload assigns
    getAssigns();
  } catch (error) {
    console.log(error);
  }
}

function handleOnSuccessNewAssignment() {
  //reload data
  getAssigns();
  data.isAddingNewAssignment = false;
}

function handleOnCancel() {
  data.isAddingNewAssignment = false;
}

async function init() {
  Promise.all([getAssigns()]).then(() => {
    data.isLoading = false;
  });
}

onMounted(() => {
  init();
});
</script>

<template>
  <NewAssign
    v-if="data.isAddingNewAssignment"
    :onSuccess="handleOnSuccessNewAssignment"
    :onCancel="handleOnCancel"
  />
  <div v-if="data.isLoading" class="loader">
    <Loader />
  </div>
  <div v-else="!data.isLoading" class="content">
    <div>
      <h1>Assignments</h1>
    </div>
    <div class="options-wrapper">
      <button class="option-button" @click="data.isAddingNewAssignment = true">
        NEW
      </button>
    </div>
    <div class="content-wrapper">
      <div class="assign-row-header">
        <p>ID</p>
        <p>PLATENUMBER</p>
        <p>BRAND</p>
        <p>MODEL</p>
        <p>YEAR</p>
        <p>DRIVER ASSIGNED</p>
      </div>
      <div v-for="assign in data.assigns" class="assign-row">
        <p>{{ assign.a_id }}</p>
        <p>{{ assign.platenumber }}</p>
        <p>{{ assign.brand }}</p>
        <p>{{ assign.model }}</p>
        <p>{{ assign.year }}</p>
        <p>{{ assign.name }} {{ assign.lastname }}</p>
        <button
          class="remove-button"
          @click="handleOnRemove({ id: assign.a_id })"
        >
          Remove
        </button>
      </div>
    </div>
  </div>
</template>
