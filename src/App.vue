<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      Log File
      <v-spacer>
      <v-text-field
        v-model="filterSearch"
        prepend-icon="fas fa-user"
        label="Search"
      />
      
      <v-select
        v-model="filterLevels"
        prepend-icon="fas fa-user-tag"
        prepend-inner=""
        :items="levels"
        attach
        chips
        label="Level"
        multiple
        item-text="label"
        item-value="id"
      />

      <v-select
        v-model="filterPpids"
        prepend-icon="fas fa-user-tag"
        prepend-inner=""
        :items="ppids"
        :disabled="ppids.length < 2"
        attach
        chips
        label="PPID"
        multiple
      />
      
      </v-spacer>

      Recherche
    </v-app-bar>

    <v-main>
      <LogReader/>
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
    filterLevels: "",
    filterLevels: [],
    filterPpids: [],
    levels: [],
    ppids: [],
  }),
};
</script>
