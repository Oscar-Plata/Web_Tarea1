<template>
      <v-navigation-drawer
        expand-on-hover 
        width="330"
        rail
        class="bg-red"
        theme="dark"
        permanent
        :elevation="5"
      >
        <v-list>
          <v-list-item
            prepend-avatar="/src/assets/avatar.png.jpg"
            class="font-weight-black"
            title="Oscar Plata"
            subtitle="lopez.oscar24@uabc.edu.mx"
          ></v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item class="text-yellow bg-black" prepend-icon="mdi-home" title="Home" value="home" color="black" to="/"></v-list-item>
          <v-list-item class="text-yellow bg-black" prepend-icon="mdi-table" title="Tabla" value="table" color="black" to="/tabla"></v-list-item>
        </v-list>
      </v-navigation-drawer>
     
</template>