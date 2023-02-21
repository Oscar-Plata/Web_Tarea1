<template>
    <v-table>
      <thead>
        <tr>
          <th class="text-left font-weight-black text-red">
            Autor
          </th>
          <th class="text-left font-weight-black text-red">
            Titulo
          </th>
          <th class="text-left font-weight-black text-red">
            Contenido
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in publicaciones"
          :key="item.titulo"
        >
          <td>{{ item.autor }}</td>
          <td>{{ item.titulo }}</td>
          <td>{{ item.texto }}</td>
        </tr>
      </tbody>
    </v-table>
  </template>

<script setup>
import { ref } from "vue";
function publicacion(autor,titulo,texto) {
  this.autor = autor;
  this.titulo = titulo;
  this.texto = texto;
}
const publicaciones = ref([]);
var postJson
var userJson
try {
  fetch('https://jsonplaceholder.typicode.com/posts/')
  .then((response) => response.json())
  .then((json)=>{postJson=json})
  .then(fetch('https://jsonplaceholder.typicode.com/users/')
  .then((response) => response.json())
  .then((json)=>{userJson=json})
  .then(async (json) => {
    if(postJson.length!=undefined){
      for(let i=0;i<postJson.length;i++){
        var uId=postJson[i].userId -1
        publicaciones.value.push(new publicacion(userJson[uId].name,postJson[i].title,postJson[i].body))
      }
    }
     
  }))
} catch (error) {
  console.log(error)
  publicaciones.value.push(new publicacion("ERROR","123","Datos no obtenidos, recargar pagina"))
}

</script>