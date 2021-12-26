<template>
  <div class="home">
    <v-text-field
        v-model="newTaskTitle"
        @click:append="addTask"
        @keyup.enter="addTask"
        class="pa-3"
        outlined
        label="Add Task"
        append-icon="mdi-plus"
        hide-details
        clearable
    ></v-text-field>

    <v-divider></v-divider>
    <v-list class="pt-0" flat>
      <div v-for="task in tasks" :key="task.id">
        <v-list-item @click="doneTask(task.id)" :class="{'blue lighten-5' : task.done}">
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title :class="{'text-decoration-line-through' : task.done}">{{task.title}}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn @click.stop="deleteTask(task.id)" icon>
                <v-icon color="primary lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>

          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
  </div>
</template>

<script>
import { db } from '../firebase/db';
import { collection } from "firebase/firestore"; 
export default {
  name: 'Home',
  data() {
    return {
      newTaskTitle: '',
      tasks: []
    }
  },
  mounted() {
    this.getTasks();
  },
  methods: {
    async getTasks() {
      let snapshot = await db.collection("tasks").get();
      let tasks = [];
      snapshot.forEach(doc => {
        let appData = doc.data();
        appData.id = doc.id;
        tasks.push(appData);
      });

      this.tasks = tasks;
    },
    async addTask() {
      if(this.newTaskTitle) {
          await db.collection('tasks').add({title: this.newTaskTitle, done: false});
      }
    },
     async addTask () {
      if(this.newTaskTitle) {
        await db.collection("tasks").add({
          title: this.newTaskTitle,
          done: false
        })
        this.newTaskTitle = '';
        this.getTasks();
      } else {
        alert('You must enter task first')
      }
    },
    async deleteTask (id) {
      await db.collection("tasks").doc(id).delete()
      this.getTasks()
    },
  },
}
</script>
