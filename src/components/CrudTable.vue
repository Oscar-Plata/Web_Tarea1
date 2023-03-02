<template>
  <v-data-table
    :headers="cabezera"
    :items="articulos"
    :sort-by="[{ key: 'id', order: 'asc' }]"
    class="mx-auto my-2 elevation-1 rounded-shaped"
    theme="dark"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Obras (CRUD)</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>

        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ props }">
            <v-btn color="red" dark class="mb-2" v-bind="props">
              Agregar
            </v-btn>
            <v-btn color="red" @click="cargarDatos()"> Reset </v-btn>
          </template>

          <v-card>
            <v-card-title>
              <span class="text-h5 text-red">{{ formTitle() }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-text-field
                    v-model="editedItem.autor"
                    label="Autor"
                  ></v-text-field>
                </v-row>
                <v-row>
                  <v-text-field
                    v-model="editedItem.autorid"
                    label="Id Autor"
                  ></v-text-field>
                </v-row>
                <v-row>
                  <v-text-field
                    v-model="editedItem.titulo"
                    label="Titulo"
                  ></v-text-field>
                </v-row>
                <v-row>
                  <v-text-field
                    v-model="editedItem.contenido"
                    label="Contenido"
                  ></v-text-field>
                </v-row>
                <v-row>
                  <v-text-field
                    v-model="editedItem.id"
                    label="Id item"
                  ></v-text-field>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="yellow-darken-1" @click="close()"> Cancelar </v-btn>
              <v-btn color="red-darken-1" @click="save()"> Guardar </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >¿Seguro que quieres borrar esto?
            </v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="yellow-darken-1" @click="closeDelete()"
                >Cancelar
              </v-btn>
              <v-btn color="red-darken-1" @click="deleteItemConfirm()">
                OK
              </v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>

    <template v-slot:item.actions="{ item }">
      <v-icon size="small" class="me-2" @click="editItem(item.raw)">
        mdi-pencil
      </v-icon>
      <v-icon size="small" @click="deleteItem(item.raw)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="red" @click="cargarDatos()"> Reset </v-btn>
    </template>
  </v-data-table>
</template>

<script setup>
import { ref, watch } from "vue";
const cabezera = ref([]);
const dialog = ref(false);
const dialogDelete = ref(false);
const articulos = ref([]);
const editedIndex = ref(-1);
const postsArray = ref([]);
const usersArray = ref([]);
const editedItem = ref({
  autor: "",
  autorid: "",
  titulo: "",
  contenido: "",
  id: "",
});
const defaultItem = ref({
  autor: "",
  autorid: "",
  titulo: "",
  contenido: "",
  id: "",
});
const fetchedItem = ref({
  autor: "",
  autorid: "",
  titulo: "",
  contenido: "",
  id: "",
});

cabezera.value = [
  { title: "Autor", align: "start", sortable: false, key: "autor" },
  { title: "Id Autor", key: "autorid" },
  { title: "Titulo", key: "titulo" },
  { title: "Contenido", key: "contenido" },
  { title: "Id", key: "id" },
  { title: "Actions", key: "actions", sortable: false },
];

function formTitle() {
  return this.editedIndex === -1 ? "Nuevo Item" : "Editar Item";
}

function editItem(item) {
  this.editedIndex = this.articulos.indexOf(item);
  this.editedItem = Object.assign({}, item);
  this.dialog = true;
}

function deleteItem(item) {
  this.editedIndex = this.articulos.indexOf(item);
  this.editedItem = Object.assign({}, item);
  this.dialogDelete = true;
}

function deleteItemConfirm() {
  this.articulos.splice(this.editedIndex, 1);
  deleteItemjs(this.editItem);
  this.closeDelete();
}

function close() {
  this.editedItem = Object.assign({}, this.defaultItem);
  this.editedIndex = -1;
  this.dialog = false;
}

function closeDelete() {
  this.editedItem = Object.assign({}, this.defaultItem);
  this.editedIndex = -1;
  this.dialogDelete = false;
}

function save() {
  if (this.editedIndex > -1) {
    Object.assign(this.articulos[this.editedIndex], this.editedItem);
    editItemjs(this.editItem);
  } else {
    this.articulos.push(this.editedItem);
    postItemjs(this.editItem);
  }
  this.close();
}

function post(id, title, body, userid) {
  this.id = id;
  this.title = title;
  this.body = body;
  this.userid = userid;
}

function user(id, name) {
  this.id = id;
  this.name = name;
}

function postItemjs(item) {
  fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    body: JSON.stringify({
      id: item.id,
      title: item.titulo,
      body: item.contenido,
      userId: item.autorid,
    }),
    headers: {
      "Content-type": "application/json; charset=UTF-8",
    },
  })
    .then((response) => response.json())
    .then((json) => console.log(json));
}

function editItemjs(item) {
  fetch("https://jsonplaceholder.typicode.com/posts/$item.id", {
    method: "PATCH",
    body: JSON.stringify({
      id: item.id,
      title: item.titulo,
      body: item.contenido,
      userId: item.autorid,
    }),
    headers: {
      "Content-type": "application/json; charset=UTF-8",
    },
  })
    .then((response) => response.json())
    .then((json) => console.log(json));
}

function deleteItemjs(item) {
  fetch("https://jsonplaceholder.typicode.com/posts/$item.id", {
    method: "DELETE",
  });
}

function cargarDatos() {
  var postJson;
  var userJson;
  fetch("https://jsonplaceholder.typicode.com/posts/")
    .then((response) => response.json())
    .then((json) => {
      postJson = json;
      for (let i = 0; i < postJson.length; i++) {
        postsArray.value.push(
          new post(
            postJson[i].id,
            postJson[i].title,
            postJson[i].body,
            postJson[i].userId
          )
        );
        //console.log(postsArray.value[i]);
      }
      console.log("post fetch");
      fetch("https://jsonplaceholder.typicode.com/users/")
        .then((response) => response.json())
        .then((json) => {
          userJson = json;
          for (let i = 0; i < userJson.length; i++) {
            usersArray.value.push(new post(userJson[i].id, userJson[i].name));
            // console.log(usersArray.value[i]);
          }
          console.log(postsArray.value.length);

          for (let i = 0; i < postsArray.value.length; i++) {
            var autorbyId = usersArray.value.find(
              (user) => user.id === postsArray.value[i].userid
            );
            var auxiliar = {
              autor: autorbyId.title,
              autorid: postsArray.value[i].userid,
              titulo: postsArray.value[i].title,
              contenido: postsArray.value[i].body,
              id: postsArray.value[i].id,
            };
            var enArray = articulos.value.find((x) => x.id === auxiliar.id);
            console.log(enArray);
            if (enArray == undefined) this.articulos.push(auxiliar);
          }
          // }
        });
    });
}
</script>
