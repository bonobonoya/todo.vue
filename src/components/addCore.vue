<template>
  <v-layout tag="div">
    <!-- add fab button -->
    <v-slide-y-reverse-transition>
      <v-btn
        v-show="!sheet"
        color="primary"
        fab
        bottom
        left
        fixed
        @click="sheet = !sheet"
      >
        <v-icon>add</v-icon>
      </v-btn>
    </v-slide-y-reverse-transition>


    <!-- bottom sheet -->
    <v-bottom-sheet
      v-model="sheet"
      hide-overlay
    >

      <!-- text field -->
      <v-text-field
        v-model="task"
        placeholder="Add task"
        solo
        hide-details
      >

        <!-- date picker on dialog -->
        <template slot="append">
          <v-dialog
            v-model="dialog"
            full-width
            width="290px"
            persistent
          >
            <v-btn slot="activator" icon>
              <v-icon :style="{color: date ? this.$vuetify.theme.primary : 'grey'}">today</v-icon>
            </v-btn>
            <v-date-picker v-model="date" no-title>
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="date = null">Reset</v-btn>
              <v-btn flat color="primary" @click="dialog = false">OK</v-btn>
            </v-date-picker>
          </v-dialog>
        </template>

        <!-- add button -->
        <template slot="append">
          <v-btn icon>
            <v-icon color="primary" @click="addTask">add_circle</v-icon>
          </v-btn>
        </template>
      </v-text-field>
    </v-bottom-sheet>
  </v-layout>
</template>

<script>
export default {
  data: () => ({
    sheet: true,
    dialog: false,
    task: '',
    date: null
  }),
  methods: {
    addTask: function () {
      this.task = '';
      this.date = null
    }
  }
}
</script>
