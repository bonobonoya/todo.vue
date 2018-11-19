<template>
  <v-container>
    <v-layout>
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
            >
              <v-btn slot="activator" icon>
                <v-icon :style="{color: date ? this.$vuetify.theme.primary : 'gray'}">today</v-icon>
              </v-btn>
              <v-date-picker
                v-model="date"
                @input="dialog = false"
                no-title
              >
                <v-spacer></v-spacer>
                <v-btn flat color="primary" @click="date = null; dialog = false;">Reset</v-btn>
              </v-date-picker>
            </v-dialog>
          </template>

          <!-- add button -->
          <template slot="append">
            <v-btn icon>
              <v-icon
                :style="{color: task ? this.$vuetify.theme.primary : 'gray'}"
                @click="addTask"
              >add_circle</v-icon>
            </v-btn>
          </template>
        </v-text-field>
      </v-bottom-sheet>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    sheet: false,
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
