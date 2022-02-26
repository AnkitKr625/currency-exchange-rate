<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
    <div>Currency Exchange Rate</div>
    </v-app-bar>

    <v-main>
      <CurrencyExchangeRate/>
    </v-main>
  </v-app>
</template>

<script>
import CurrencyExchangeRate from './components/CurrencyExchangeRate';

export default {
  name: 'App',

  components: {
    CurrencyExchangeRate,
  },

  data: () => ({
    //
  }),
};
</script>
