<template>
  <v-app>
    <v-main>
      <Weather/>
    </v-main>
    <Footer />
  </v-app>
</template>

<script>
import Weather from './components/Weather';
import Footer from './components/Footer';

export default {
  name: 'App',

  components: {
    Weather,
    Footer
  },

  data: () => ({
    //
  }),
};
</script>
<style lang="scss" scoped>
#app {
  background: whitesmoke;
}
</style>