<template>
<v-app id="inspire">
  <v-navigation-drawer 
  v-model="drawer" 
  :mobile-breakpoint="768"
  app
  >

    <v-img
    class="pa-4 pt-7"
      src="mountains.png"
      height="170"
      gradient="to top right, rgba(55,236,186,.7), rgba(25,32,72,.7)"
    >
    <v-avatar size="70" class="mb-2">
      <img
        src="https://cdn.vuetifyjs.com/images/john.jpg"
        alt="John"
      >
    </v-avatar>
    <div class="white--text text-subtitle-1 font-weight-bold">
      Stan Smith
      </div>
    <div class="white--text text-subtitle-2">
      s_smith
      </div>
    </v-img>

    <v-list
        dense
        nav
    >
      <v-list-item
          v-for="item in items"
          :key="item.title"
          :to="item.to"
          link
      >
        <v-list-item-icon>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-item-icon>

        <v-list-item-content>
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>

  </v-navigation-drawer>

  <v-app-bar
      app
      color="primary"
      dark
      prominent
      src="mountains.png"
      prominent
      :height="$route.path === '/' ? '238' : '170'"
  >
    <template v-slot:img="{ props }">
      <v-img
          v-bind="props"
          gradient="to top right, rgba(55,236,186,.9), rgba(25,32,72,.9)"
      ></v-img>
    </template>

    <v-container class="header-container pa-2">
      <v-row>
        <v-app-bar-nav-icon @click="drawer= !drawer"></v-app-bar-nav-icon>
        <v-spacer></v-spacer>
        <search />
      </v-row>
       <v-row>
         <v-toolbar-title class="text-4 ml-4">
           To Do
         </v-toolbar-title>
      </v-row>
      <v-row>
       <live-date-time />
      </v-row>
      <v-row v-if="$route.path === '/'">
        <field-add-task />
      </v-row>
    </v-container>

  </v-app-bar>

<v-main>
  <router-view></router-view>
  <snackbar />
</v-main>
</v-app>
</template>

<script>
import Snackbar from '@/components/Global/Snackbar.vue'
import Search from '@/components/Tools/Search.vue'
import LiveDateTime from '@/components/Tools/LiveDateTime.vue'
import FieldAddTask from './components/Tasks/FieldAddTask.vue'

export default {
  data: () => ({
    drawer: null,
    items: [
      { title: 'Tasks', icon: 'mdi-format-list-checks', to: '/'},
      { title: 'About', icon: 'mdi-help-box', to: '/about'},
    ],
  }),
  mounted() {
    this.$store.dispatch('getTasks')
  },
  components: {
    'search': Search,
    'snackbar': Snackbar,
    'live-date-time': LiveDateTime,
    'field-add-task': FieldAddTask
  }
}
</script>

<style lang="sass">
  .header-container
    max-width: none !important
</style>