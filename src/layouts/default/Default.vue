<template>
  <v-app>
    <default-bar />
<sidebar></sidebar>
    <default-view />
  </v-app>
</template>

<script setup>
  import DefaultBar from './AppBar.vue'
  import DefaultView from './View.vue'
  import Sidebar from './Sidebar.vue';
</script>
