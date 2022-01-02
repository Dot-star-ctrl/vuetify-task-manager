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
    <v-list 
    v-if="tasks.length"
    class="pt-0" 
    flat>
      <div v-for="task in tasks" :key="task.id">
        <v-list-item
          :class="{ 'blue lighten-5': task.done }"
        >

          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done" @click="doneTask(task.id)" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
                >{{ task.title }}</v-list-item-title
              >
              </v-list-item-content>

            <v-list-item-action>
              <v-btn @click="deleteTask(task.id)" icon>
                <v-icon color="primary lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>

          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
    <div
    v-else
    class="no-tasks"
    >
    <v-icon
    size="100"
    color="primary">
    mdi-check
    </v-icon>
    <div class="text-h5 primary--text">No tasks</div>
    </div>
  </div>
</template>

<script>
import { db } from "../firebase/db";
export default {
  name: "Home",
  data() {
    return {
      newTaskTitle: "",
      tasks: [],
      dialog: false,
      selectedTask: '',
      deteleTaskId: '',
    };
  },
  mounted() {
    this.getTasks();
  },
  methods: {
    async getTasks() {
      let snapshot = await db.collection("tasks").get();
      let tasks = [];
      snapshot.forEach((doc) => {
        let appData = doc.data();
        appData.id = doc.id;
        tasks.push(appData);
      });

      this.tasks = tasks;
    },
    async addTask() {
      if (this.newTaskTitle) {
        await db
          .collection("tasks")
          .add({
            title: this.newTaskTitle,
            done: false,
          })
          .then((docRef) => {
            console.log("Document written with ID: ", docRef.id);
            this.newTaskTitle = "";
            this.getTasks();
          })
          .catch((error) => {
            console.error("Error adding document: ", error);
          });
      } else {
        alert("You must enter task first");
      }
    },
    async deleteTask(id) {
      console.log(id);
      await db.collection("tasks").doc(id).delete().then(() => {
          console.log("Document successfully deleted!");
          this.getTasks();
          this.dialog = false;
      }).catch((error) => {
          console.error("Error removing document: ", error);
      });
    
    },
    async doneTask(id) {
      var taskRef = await db.collection("tasks").doc(id);

      taskRef
        .get()
        .then((doc) => {
          if (doc.exists) {
            taskRef
              .update({
                done: !doc.data().done,
              })
              .then(() => {
                this.getTasks();
                console.log("Document successfully updated!");
              });
          } else {
            // doc.data() will be undefined in this case
            console.log("No such document!");
          }
        })
        .catch((error) => {
          console.log("Error getting document:", error);
        });
    },
  },
};
</script>

<style lang="sass">
.no-tasks
  position: absolute
  left: 50%
  top: 50%
  transform: translate(-50%, -50%)
  opacity: 0.5
</style>
