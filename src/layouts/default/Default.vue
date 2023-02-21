<template>
  <v-app>
    <AppBar/>
    <SideMenu />
    <default-view />
  </v-app>
</template>

<script setup>
import AppBar from './AppBar.vue';
import SideMenu from './SideMenu.vue';
  import DefaultView from './View.vue'
</script>
