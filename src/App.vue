<template>
  <v-app>
    <v-content>
      <todo/>
    </v-content>
  </v-app>
</template>

<script>
import todo from '@/components/todo';

export default {
  name: 'App',

  components: {
    todo
  },

  data: () => ({
    //
  }),
};
</script>