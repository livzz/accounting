<template>
  <div id="app">
      <app-landing-page v-if="$route.path !== '/print' && $route.path !== '/edit' "></app-landing-page>
    <router-view></router-view>
  </div>
</template>

<script>
/* eslint-disable no-trailing-spaces,indent */
import LandingPage from "./components/LandingPage";

export default {
  name: "elevue",
  components: {
    appLandingPage: LandingPage,
  },
};
</script>

<style>
/* CSS */
html, body {
      height: 100%;
      overflow: hidden;
   }
</style>
