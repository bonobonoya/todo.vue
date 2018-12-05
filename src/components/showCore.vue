<template>
  <v-container ma-0 pa-0>
    <v-layout row>
      <v-flex>

        <!-- tab -->
        <v-tabs-items :value.sync="tab">
          <v-flex xs12 lg5>

            <!-- tab items -->
            <v-tab-item value="tab-todos">

              <!-- expansion panel for tasks -->
              <v-expansion-panel v-model="tasksPanel" focusable>
                <v-expansion-panel-content
                  v-for="(task, index) in tasksList"
                  :key="'panel-' + index"
                >

                  <!-- task header -->
                  <div slot="header">
                    <v-layout justify-center align-center>
                      <span class="display-1 font-weight-medium">{{ task.task }}</span>
                      <v-spacer></v-spacer>
                      <span class="subheading grey--text pr-4">
                        {{ task.date }}
                      </span>
                    </v-layout>
                  </div>

                  <!-- task control ui -->
                  <v-card class="grey lighten-3">
                    <v-layout align-center>
                      <v-btn
                        color="success"
                        block
                        depressed
                        @click="tasksList[index].status = 'completed'; tasksPanel = null"
                      >
                        <v-icon>done</v-icon>
                      </v-btn>
                      <v-btn
                        color="primary"
                        block
                        depressed
                        @click="editProcess(tasksList[index])"
                      >
                        <v-icon>edit</v-icon>
                      </v-btn>
                      <v-btn
                        color="error"
                        block
                        depressed
                        @click="deleteProcess(tasksList[index])"
                      >
                        <v-icon>delete_forever</v-icon>
                      </v-btn>
                    </v-layout>
                  </v-card>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-tab-item>

            <!-- expansion panel for completed tasks -->
            <v-tab-item value="tab-completed">
              <v-expansion-panel v-model="completedPanel" focusable>
                <v-expansion-panel-content
                  v-for="(task, index) in completedList"
                  :key="'panel-' + index"
                  ripple
                >

                  <!-- task header -->
                  <div slot="header">
                    <v-layout justify-center align-center>
                      <span class="display-1 font-weight-medium">{{ task.task }}</span>
                      <v-spacer></v-spacer>
                      <span class="subheading grey--text text-no-wrap pr-4">
                        {{ task.date }}
                      </span>
                    </v-layout>
                  </div>

                  <!-- task control ui -->
                  <v-card class="grey lighten-3">
                    <v-layout align-center>
                      <v-btn
                        color="warning"
                        @click="completedList[index].status = 'task'; completedPanel = null"
                        block
                        depressed
                      >
                        <v-icon>restore</v-icon>
                      </v-btn>
                      <v-btn
                        color="primary"
                        block
                        depressed
                        @click="editProcess(completedList[index])"
                      >
                        <v-icon>edit</v-icon>
                      </v-btn>
                      <v-btn
                        color="error"
                        block
                        depressed
                        @click="deleteProcess(completedList[index])"
                      >
                        <v-icon>delete_forever</v-icon>
                      </v-btn>
                    </v-layout>
                  </v-card>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-tab-item>
          </v-flex>
        </v-tabs-items>
      </v-flex>

      <!-- bottom sheet -->
      <v-bottom-sheet
        v-model="sheet"
        hide-overlay
      >
        <todo-input :edit="true">
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
  props: ['tab', 'tabItems'],
  data() {
    return {
      tasksPanel: null,
      completedPanel: null,
      sheet: false,
      tasks: []
    }
  },
  created() {
    let self = this;
    eventBus.$on('addTask', function (data) {
      self.tasks.push({
        task: data.task,
        date: data.date,
        status: 'task'
      });
    }),
      eventBus.$on('editTask', function (data) {
        let target = self.tasks[self.tasks.indexOf(data.old)];
        target.task = data.new.task;
        target.date = data.new.date;
      });
    // eventBus.$on('sheetOn', function () {
    //   self.sheet = true;
    // });
    eventBus.$on('sheetOff', function () {
      self.sheet = false;
    });
  },
  mounted() {
    let data = window.localStorage.getItem('tasks');
    if (data) {
      try {
        this.tasks = JSON.parse(data);
      } catch (e) {
        window.localStorage.removeItem('tasks');
      }
    }
  },
  watch: {
    sheet: function () {
      if (this.sheet) {
        eventBus.$emit('fabOff');
      } else {
        eventBus.$emit('fabOn');
      }
    },
    tasks: {
      handler: function () {
        window.localStorage.setItem('tasks', JSON.stringify(this.tasks));
      },
      deep: true
    }
  },
  computed: {
    tasksList: function () {
      return this.tasks.filter(function (obj) {
        return obj.status === 'task'
      })
    },
    completedList: function () {
      return this.tasks.filter(function (obj) {
        return obj.status === 'completed'
      })
    }
  },
  methods: {
    deleteProcess: function (obj) {
      this.tasks.splice(this.tasks.indexOf(obj), 1);
      this.tasksPanel = null;
      this.completedPanel = null;
    },
    editProcess: function (obj) {
      eventBus.$emit('passTask', obj);
      this.sheet = true;
    }
  }
}
</script>
