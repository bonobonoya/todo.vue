<template>
  <v-container>
    <v-layout>
      <!-- add fab button -->
      <v-slide-y-reverse-transition>
        <v-btn
          v-show="(!sheet) && fab"
          color="primary"
          fab
          bottom
          left
          fixed
          @click="sheet = true"
        >
          <v-icon>add</v-icon>
        </v-btn>
      </v-slide-y-reverse-transition>


      <!-- bottom sheet -->
      <v-bottom-sheet
        v-model="sheet"
        hide-overlay
      >
        <todo-input>
        </todo-input>
      </v-bottom-sheet>
    </v-layout>
  </v-container>
</template>

<script>
import todoInput from './todoInput';
import { eventBus } from '../main';

export default {
  components: {
    todoInput
  },
  props: ['tab'],
  data() {
    return {
      sheet: false,
      fab: true
    }
  },
  created() {
    let self = this
    eventBus.$on('fabOn', function () {
      self.fab = true;
    });
    eventBus.$on('fabOff', function () {
      self.fab = false;
    });
    // eventBus.$on('sheetOn', function () {
    //   self.sheet = true;
    // });
    eventBus.$on('sheetOff', function () {
      self.sheet = false;
    });
  },
  watch: {
    tab: function () {
      this.sheet = false;
    }
  }
}
</script>
