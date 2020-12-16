<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-btn @click="browseLogFiles()" >
        <v-icon left dark >
          far fa-folder-open
        </v-icon>
        PROD / api.log
      </v-btn>
      <v-btn icon @click="switchRefreshMode()" >
        <v-icon>fas fa-sync</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-text-field
        hide-details
        v-model="filterSearch"
        prepend-icon="fas fa-search"
        label="Search"
      />
      
      

      <v-spacer></v-spacer>
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            dark
            v-bind="attrs"
            v-on="on"
          >
            Advanced
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title>Colapse all</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>Filter logs</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>Download logs</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>

    <v-main>
      <LogReader :quickSearch="filterSearch"/>
    </v-main>
  </v-app>
</template>

<script>
import LogReader from './components/LogReader';

export default {
  name: 'App',

  components: {
    LogReader,
  },

  data: () => ({
    filterSearch: "",
    filterLevels: [],
    filterPpids: [],
    levels: [],
    ppids: [],

  }),
  methods: {
  }
};
</script>
