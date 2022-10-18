<script lang="ts">
import MainScreen from "./components/MainScreen.vue";
import Drivers from "./components/Drivers.vue";
import Vehicles from "./components/Vehicles.vue";
import Home from "./components/Home.vue";

const routes: any = {
  "/": Home,
  "/assignments": MainScreen,
  "/vehicles": Vehicles,
  "/drivers": Drivers,
};

export default {
  data() {
    return {
      currentPath: window.location.hash,
    };
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || "/"];
    },
  },
  mounted() {
    window.addEventListener("hashchange", () => {
      this.currentPath = window.location.hash;
    });
  },
};
</script>
<template>
  <div class="navigation">
    <div class="nav-logo" />
    <a href="#/">Home</a>
    <a href="#/assignments">Assignments</a>
    <a href="#/drivers">Drivers</a>
    <a href="#/vehicles">Vehicles</a>
  </div>
  <component :is="currentView" />
</template>

<style scoped>
.nav-logo {
  height: 60px;
  width: 60px;
  border-radius: 50%;
  background-image: url("assets/images/logo.png");
  background-size: 200%;
  background-position: center center;
  margin-right: 20px;
}
</style>
