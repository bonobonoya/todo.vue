<template>
  <!-- text field -->
  <v-text-field
    v-model="task"
    placeholder="Add task"
    solo
    hide-details
    @keyup.enter="!editOld ? addTask() : editTask()"
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
    <template v-if="!editOld" slot="append">
      <v-btn icon>
        <v-icon
          :style="{color: task ? this.$vuetify.theme.primary : 'gray'}"
          @click="addTask"
        >add_circle</v-icon>
      </v-btn>
    </template>
    <template v-else slot="append">
      <v-btn icon>
        <v-icon
          :style="{color: task ? this.$vuetify.theme.primary : 'gray'}"
          @click="editTask"
        >edit</v-icon>
      </v-btn>
    </template>
  </v-text-field>
</template>

<script>
import { eventBus } from '../main';

export default {
  props: ['edit'],
  data: () => ({
    dialog: false,
    editOld: null,
    task: '',
    date: null
  }),
  created() {
    let self = this;
    if (this.edit) {
      eventBus.$on('passTask', function (obj) {
        self.task = obj.task;
        self.date = obj.date;
        self.editOld = obj
      });
    }
  },
  methods: {
    addTask: function () {
      eventBus.$emit('addTask', {
        task: this.task,
        date: this.date
      })
      this.editOld = null
      this.task = '';
      this.date = null
      eventBus.$emit('sheetOff');
    },
    editTask: function () {
      eventBus.$emit('editTask', {
        old: this.editOld,
        new: {
          task: this.task,
          date: this.date
        }
      })
      this.editOld = null;
      this.task = '';
      this.date = null
      eventBus.$emit('sheetOff');
    }
  }
}
</script>
