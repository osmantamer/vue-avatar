<template>
  <v-app>
    <v-main>
      <Avatar/>
    </v-main>
  </v-app>
</template>

<script>
import Avatar from './components/Avatar';

export default {
  name: 'App',

  components: {
    Avatar
  },

  data: () => ({
    //
  }),
};
</script>
