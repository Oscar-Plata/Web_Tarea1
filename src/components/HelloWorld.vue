<template>
  <v-container>
    <v-row justify="space-around">
      <v-card width="800">
        
        <v-img height="200" :src="url" cover class="text-white">
          <v-toolbar color="rgba(0, 0, 0, 0)" theme="dark">
            <template v-slot:prepend>
              <v-btn icon="$menu"></v-btn>
            </template>

            <v-toolbar-title class="text-h6"> Mensajes </v-toolbar-title>

            <template v-slot:append>
              <v-btn icon="mdi-dots-vertical"></v-btn>
            </template>
          </v-toolbar>
          <template v-slot:placeholder>
      <div class="d-flex align-center justify-center fill-height">
        <v-progress-circular
          color="grey-lighten-4"
          indeterminate
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
const url = ref("https://random.imagecdn.app/500/200");
const red = ref("cyan");
messages.value = [
  {
    from: "You",
    message: `Sure, I'll see you later.`,
    time: "10:42am",
    color: "deep-purple-lighten-1",
  },
  {
    from: "John Doe",
    message: "Yeah, sure. Does 1:00pm work?",
    time: "10:37am",
    color: "green",
  },
  {
    from: "You",
    message: "Did you still want to grab lunch today?",
    time: "9:47am",
    color: "blue",
  },
];

async function recargarImagen() {
  let respuesta = await fetch("https://random.imagecdn.app/500/200");

  url.value = respuesta.url;
  console.log(respuesta.url);
}
</script>
