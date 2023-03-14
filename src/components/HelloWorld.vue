<template>
  <v-container>
    <v-row justify="space-around">
      <v-card width="750" class="rounded-shaped">
        <v-img height="200" :src="url" cover class="text-white" :elevation="20">
          <v-toolbar color="rgba(0, 0, 0, 0)" theme="dark">
            <v-toolbar-title class="text-h4 mb"> Mensajes </v-toolbar-title>
          </v-toolbar>
          <template v-slot:placeholder>
            <div class="d-flex align-center justify-center fill-height">
              <v-progress-circular
                indeterminate
                color="red"
              ></v-progress-circular>
            </div>
          </template>
        </v-img>

        <v-card-text>
          <div class="font-weight-bold ms-1 mb-2">Hoy</div>

          <v-timeline density="compact" align="start">
            <v-timeline-item
              v-for="message in messages"
              :key="message.time"
              :dot-color="message.color"
              size="x-large"
            >
              <div class="mb-4">
                <div class="font-weight-normal">
                  <strong>{{ message.from }}</strong> @{{ message.time }}
                </div>
                <div>{{ message.message }}</div>
              </div>
            </v-timeline-item>
          </v-timeline>
        </v-card-text>
        <v-card-actions>
          <v-btn @click="messages = []" color="red">BORRAR</v-btn>
          <v-btn @click="recargarImagen()" :color="red">RECARGAR</v-btn>
        </v-card-actions>
      </v-card>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref } from "vue";
const messages = ref([]);
const url = ref(
  "https://img.freepik.com/vector-gratis/efecto-superposicion-chispas-fuego-rojo-fogata-ardiente_107791-13792.jpg?w=826&t=st=1677200697~exp=1677201297~hmac=c4c7725f488b84de7300f144378feb39b10015985caefcaca71b55a8c9cddee6"
);
const red = ref("black");
messages.value = [
  {
    from: "Yo",
    message: `Hola, esteban 🐸`,
    time: "10:42am",
    color: "red",
  },
  {
    from: "Esteban",
    message: "Alo Oscar 🐢",
    time: "10:37am",
    color: "yellow",
  },
];

async function recargarImagen() {
  let respuesta = await fetch("https://random.imagecdn.app/500/200");

  url.value = respuesta.url;
  console.log(respuesta.url);
}
</script>
