<template>
  <v-btn @click.stop="evento">{{ etiqueta }}</v-btn>
  <!-- <v-btn @click.stop="$emit('cambio', ++numero)">{{ etiqueta }}</v-btn> -->
</template>

<script setup>
import { ref } from "vue";
const numero = ref(1);
const props = defineProps({ etiqueta: String });
const etiqueta = ref(props.etiqueta);
// console.log(etiqueta.value);
// const mensajes = ref(["Primer", "Segundo", "Tercero"]);

// function rndMns() {
//   return mensajes.value[Math.floor(Math.random() + 3)];
// }
const emitir = defineEmits(["cambio"]);
function evento() {
  emitir("cambio", ++numero.value);
}
</script>
