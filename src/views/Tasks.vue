<template>
  <div class="home">
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
import ListTasks from '@/components/Tasks/ListTasks.vue'
import NoTasks from '@/components/Tasks/NoTasks.vue'
import ButtonDoneSorting from '@/components/Tasks/ButtonDoneSorting.vue'

export default {
  name: "Home",
  components: {
    'list-tasks': ListTasks,
    'no-tasks': NoTasks,
    'button-done-sorting': ButtonDoneSorting
  }
};
</script>
