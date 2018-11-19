<template>
  <v-app class="grey lighten-2">
    <!-- <v-navigation-drawer
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
    </v-navigation-drawer> -->

    <v-toolbar color="primary" dark fixed app>
      <!-- <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon> -->
      <v-toolbar-title>Todo Main</v-toolbar-title>
       <v-tabs
        slot="extension"
        v-model="tab"
        color="primary"
        grow
      >
        <v-tabs-slider color="white"></v-tabs-slider>

        <v-tab v-for="item in items"
          :key="item"
          :href="`#tab-${item}`"
        >{{ item }}
        </v-tab>
      </v-tabs>
    </v-toolbar>

    <v-content>
      <show-core :tab="tab"></show-core>
      <add-core></add-core>
    </v-content>
  </v-app>
</template>

<script>
  import showCore from './components/showCore'
  import addCore from './components/addCore'

export default {
  components: {
    showCore,
    addCore
  },
  data() {
    return {
      // drawer: null,
      tab: null,
      items: [
        'Todos',
        'Completed'
      ]
    }
  }
}
</script>
