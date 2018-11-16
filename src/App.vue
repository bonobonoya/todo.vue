<template>
  <v-app>
    <v-navigation-drawer
      fixed
      v-model="drawer"
      app
    >
      <v-list dense>
        <v-list-tile @click="">
          <v-list-tile-action>
            <v-icon>home</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Home</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar color="primary" dark fixed app>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>Todo Main</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <core></core>
    </v-content>
  </v-app>
</template>

<script>
import core from './components/core';

export default {
  components: {
    core
  },
  data() {
    return {
      drawer: null
    }
  }
}
</script>
