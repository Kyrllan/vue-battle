<template>
  <v-app>
    <v-main>
      <Battle/>
    </v-main>
  </v-app>
</template>

<script>
import Battle from './components/Battle'

export default {
  name: 'App',

  components: {
    Battle
  },

  data: () => ({
    //
  })
}
</script>
