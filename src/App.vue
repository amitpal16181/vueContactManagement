<template>
  <!-- <LoadSpinner /> -->
  <NavBar />
  <router-view />
</template>

<script>

import NavBar from "@/components/NavBar.vue";
// import LoadSpinner from "@/components/LoadSpinner.vue";
export default {
  name: 'App',
  components: { NavBar }        // Add components here as well
}
</script>

<style></style>
