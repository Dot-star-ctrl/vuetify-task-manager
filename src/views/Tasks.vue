<template>
  <div class="home">

    <field-add-task />

    <list-tasks 
      v-if="$store.state.tasks.length"
    />
   
    <no-tasks
      v-else 
    />

    <button-done-sorting 
    v-if="$store.state.sorting"
    />

  </div>
</template>

<script>
import FieldAddTask from '@/components/Tasks/FieldAddTask.vue'
import ListTasks from '@/components/Tasks/ListTasks.vue'
import NoTasks from '@/components/Tasks/NoTasks.vue'
import ButtonDoneSorting from '@/components/Tasks/ButtonDoneSorting.vue'

export default {
  name: "Home",
  components: {
    'field-add-task': FieldAddTask,
    'list-tasks': ListTasks,
    'no-tasks': NoTasks,
    'button-done-sorting': ButtonDoneSorting
  }
};
</script>
