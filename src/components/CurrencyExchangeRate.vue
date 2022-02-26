<template>
  <v-app>
    <v-container>
      <v-row align="center">
        <v-col cols="12" sm="4" md="3">
          <v-text-field
            v-model="baseCurrencyValue"
            placeholder="Base Currency"
            type="Number"
            outlined
          ></v-text-field>
        </v-col>
        <v-col class="d-flex" cols="6" sm="4">
          <v-select
            v-model="selectedBaseCurrency"
            :items="currencies.default.data"
            label="Base Currency"
            outlined
          ></v-select>
        </v-col>
      </v-row>
      <v-row align="center">
        <v-col cols="12" sm="4" md="3">
          <v-text-field
            v-model="exchangeCurrencyValue"
            placeholder="Exchange Currency"
            type="Number"
            outlined
          ></v-text-field>
        </v-col>
        <v-col class="d-flex" cols="6" sm="4">
          <v-select
            v-model="selectedExchangeCurrency"
            :items="currencies.default.data"
            label="Exchange Currency"
            outlined
          ></v-select>
        </v-col>
      </v-row>
      <!-- <v-btn
        @click="fetchRate"
        color="primary"
      >Fetch Rate</v-btn> -->
    </v-container>
  </v-app>
</template>

<script>
import * as curr from "@/dataproviders/data.json";
export default {
  name: "CurrencyExchangeRate",
  data() {
    return {
      currencies: null,
      selectedBaseCurrency: "USD",
      selectedExchangeCurrency: "INR",
      currenciesRate: {},
      exchangeCurrencyValue: null,
      baseCurrencyValue: 1,
      rate: 0,
    };
  },
  watch: {
    selectedExchangeCurrency(newV) {
      this.rate = this.currenciesRate.data[newV];
      this.exchangeToBase(this.rate);
    },
    selectedBaseCurrency() {
      this.fetchRate();
    },
    baseCurrencyValue() {
      this.baseToExchange(this.rate)
    },
    exchangeCurrencyValue() {
      this.exchangeToBase(this.rate)
    }
  },
  created() {
    this.currencies = curr;
    this.fetchRate();
    this.baseToExchange(this.rate)
  },
  methods: {
    fetchRate() {
      let axios = require("axios");

      let config = {
        method: "get",
        url: `https://freecurrencyapi.net/api/v2/latest`,
        params: {
          apikey: `e9bb2ca0-94cf-11ec-868e-2d9b3cdc1855`,
          base_currency: `${this.selectedBaseCurrency}`,
        }
      };

      axios(config)
        .then((response) => {
          this.currenciesRate = response.data;
          this.rate = this.currenciesRate.data[this.selectedExchangeCurrency];
          this.exchangeToBase(this.rate);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    baseToExchange(rate) {
      this.exchangeCurrencyValue = this.baseCurrencyValue * rate;
    },
    exchangeToBase(rate) {
      this.baseCurrencyValue = this.exchangeCurrencyValue / rate;
    }
  },
};
</script>

<style scoped>
/deep/ .v-text-field.v-text-field--enclosed .v-text-field__details {
  padding-top: 0px;
  display: none;
}
</style>