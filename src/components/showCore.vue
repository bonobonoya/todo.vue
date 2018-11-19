<template>
  <v-container ma-0 pa-0>
    <v-layout row>
      <v-flex>

        <!-- <v-list two-line class="pb-0">
          <template v-for="(task, index) in tasks">
            <v-list-tile
              :key="'task-' + index"
              avatar
              ripple
              @click=""
            >
              <v-list-tile-content>
                <v-list-tile-title class="title">{{ task.task }}</v-list-tile-title>
              </v-list-tile-content>
              <v-spacer></v-spacer>
              <div class="grey--text">
                {{ task.date }}
              </div>
            </v-list-tile>
            <v-divider
              :key="'div-' + index">
            </v-divider>
          </template>
        </v-list> -->

        <v-tabs-items v-model="tab">
          <v-tab-item
            v-for="item in tabItems"
            :id="'tab-' + item"
            :key="'tab-content-' + item"
          >
            <v-flex xs12 lg5>
              <v-expansion-panel>
                <v-expansion-panel-content
                  v-for="(task, index) in showList"
                  :key="'panel-' + index"
                  ripple
                >
                  <div slot="header">
                    <v-layout justify-center align-center>
                      <span class="display-1 font-weight-medium">{{ task.task }}</span>
                      <v-spacer></v-spacer>
                      <span class="subheading grey--text text-no-wrap pr-4">
                        {{ task.date }}
                      </span>
                    </v-layout>
                  </div>
                  <v-card class="grey lighten-3">
                    <v-layout align-center>
                      <v-btn color="success" block depressed>
                        <v-icon>done</v-icon>
                      </v-btn>
                      <v-btn color="primary" block depressed>
                        <v-icon>create</v-icon>
                      </v-btn>
                      <v-btn color="error" block depressed>
                        <v-icon>delete_forever</v-icon>
                      </v-btn>
                    </v-layout>
                  </v-card>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-flex>
          </v-tab-item>
        </v-tabs-items>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  props: ['tab', 'tabItems'],
  data() {
    return {
      tasks: [{
        task: 'test1',
        date: '2018-11-15',
        status: 'task'
      },
      {
        task: 'test2',
        date: '2018-11-18',
        status: 'task'
      },
      {
        task: 'test3',
        date: '2018-11-05',
        status: 'completed'
      },
      {
        task: 'test4',
        date: '2019-11-11',
        status: 'task'
      },
      {
        task: 'test5',
        date: '2017-09-03',
        status: 'task'
      }],
      showList: null
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
  watch: {
    tab: function () {
      this.showList = this.tab === 'tab-todos' ? this.tasksList : this.completedList
    }
  }
}
</script>
