<template>
  <v-btn
  @click="$store.commit('toggleSorting')"
  class="button-done-sorting"
  color="primary"
  >
      Done Sorting
  </v-btn>
</template>

<style lang="sass">
    .button-done-sorting
      position: fixed
      bottom: 20px
      left: 50%
      transform: translateX(-50%)
</style>