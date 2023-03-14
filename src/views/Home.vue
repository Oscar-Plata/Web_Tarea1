<template>
  <HelloWorld />
  <v-card class="pa-6 rounded-shaped" width="750" centered :elevation="20">
    <v-row justify="space-around">Contador:{{ valor }}</v-row>
    <v-row justify="space-around">
      <botonDesplegable
        @cambio="(e) => (valor = e)"
        :etiqueta="etiqueta"
      ></botonDesplegable>
    </v-row>
  </v-card>
</template>

<script setup>
import { ref } from "vue";
import HelloWorld from "@/components/HelloWorld.vue";
import botonDesplegable from "@/components/botonDesplegable.vue";
const etiqueta = ref("Aumentar");
const valor = ref(1);
</script>
