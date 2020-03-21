<template>
  <v-container>
    <v-row class="text-center" align="center" justify="center">
      <v-col cols="12">
        <Logo />
        <h1 style="color:rgb(33, 43, 54)">Covid-19 Coronavirus Outbreak</h1>
        <p>
          Data sources from
          <a
            class="tiya"
            href="https://github.com/mathdroid/covid-19-api"
            target="_blank"
          >Muhammad Mustadi's</a> API
          <br />Designed by
          <a
            class="tiya"
            href="https://github.com/alankilalank"
            target="_blank"
          >Alank Ilalank</a>. Developed by
          <a
            class="tiya"
            href="https://github.com/princerafid01"
            target="_blank"
          >Mahmud Rafid</a>.
        </p>
      </v-col>
      <v-spacer />
      <v-col cols="12">
        <h1 style="color:rgb(33, 43, 54)">Global Cases</h1>
        <p class="tiya">Last Updated: {{ global.lastUpdate }}</p>
      </v-col>
      <v-col cols="12" md="3">
        <v-card class="mx-auto pt-3 pb-3" shaped :loading="globalLoading">
          <v-card-text class="white">
            <p class="display-2 orange--text">{{ global.confirmed | putComma }}</p>
            <h2>Confirmed</h2>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="3">
        <v-card class="mx-auto pt-3 pb-3" shaped :loading="globalLoading">
          <v-card-text class="white">
            <p class="display-2" style="color:rgb(236, 49, 75)">{{ global.deaths | putComma }}</p>
            <h2>Death</h2>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="3">
        <v-card class="mx-auto pt-3 pb-3" shaped :loading="globalLoading">
          <v-card-text class="white">
            <p class="display-2" style="color:rgb(5, 181, 132)">{{ global.recovered | putComma }}</p>
            <h2>Recovered</h2>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <!-- Another section -->

    <v-row class="text-center" align="center" justify="center">
      <v-col cols="12">
        <v-spacer />
      </v-col>
      <v-col cols="12">
        <h1 style="color:rgb(33, 43, 54)">Search for Country's Update</h1>
      </v-col>
      <!-- Country value -->
      <v-col cols="12">
        <section class="country col-md-3 text-center mx-auto">
          <model-select
            :options="options"
            @input="changeCountry"
            v-model="selectedCountry"
            placeholder="Select Country"
          ></model-select>
        </section>
      </v-col>
      <v-alert type="error" v-if="countryError">Oops! The country's information could not be found.</v-alert>
      <v-col cols="12">
        <p class="tiya">Last Updated: {{ countriesUpdate.lastUpdate }}</p>
      </v-col>
      <v-col cols="12" md="3">
        <v-card class="mx-auto pt-3 pb-3" shaped :loading="loading">
          <v-card-text class="white">
            <p class="display-2 orange--text">{{ countriesUpdate.confirmed | putComma }}</p>
            <h2>Confirmed</h2>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="3">
        <v-card class="mx-auto pt-3 pb-3" shaped :loading="loading">
          <v-card-text class="white">
            <p
              class="display-2"
              style="color:rgb(236, 49, 75)"
            >{{ countriesUpdate.deaths | putComma}}</p>
            <h2>Death</h2>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="3">
        <v-card class="mx-auto pt-3 pb-3" shaped :loading="loading">
          <v-card-text class="white">
            <p
              class="display-2"
              style="color:rgb(5, 181, 132)"
            >{{ countriesUpdate.recovered | putComma }}</p>
            <h2>Recovered</h2>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Logo from "./SvgLogo";
import axios from "axios";
import moment from "moment";
import countries from "./countries.json";
import { ModelSelect } from "vue-search-select";

export default {
  name: "Home",
  components: { Logo, ModelSelect },

  data: () => ({
    data: "",
    countryError: false,
    loading: false,
    globalLoading: false,
    options: [],
    api_key: "69147e9b0fa48fea1bcd1a45d2aa0664168d849ba7a6c5d6150c4f81", // free account
    selectedCountry: {
      value: "",
      text: ""
    },
    baseEndpoint: "https://covid19.mathdro.id/api",
    global: {
      confirmed: 0,
      deaths: 0,
      recovered: 0,
      lastUpdate: ""
    },
    countriesUpdate: {
      confirmed: 0,
      deaths: 0,
      recovered: 0,
      lastUpdate: ""
    }
  }),
  methods: {
    async locatedCountry() {
      let { data } = await axios.get(
        `https://api.ipdata.co/?api-key=${this.api_key}`
      );
      this.selectedCountry.value = data.country_code;
      this.selectedCountry.text = data.country_name;
      this.changeCountry();
    },
    async getRootData() {
      this.globalLoading = true;
      const response = await axios.get(this.baseEndpoint);
      this.data = response.data;
      console.log(this.data);
      this.global.confirmed = this.data.confirmed.value;
      this.global.deaths = this.data.deaths.value;
      this.global.recovered = this.data.recovered.value;
      this.global.lastUpdate = moment(this.data.lastUpdate).format(
        "MMMM Do YYYY, h:mm:ss a"
      );
      this.globalLoading = false;
    },
    async getCountries() {
      for (let [text, value] of Object.entries(countries)) {
        this.options.push({ text, value });
      }
    },
    async changeCountry() {
      this.countryError = false;
      this.loading = true;
      const { data } = await axios
        .get(`${this.baseEndpoint}/countries/${this.selectedCountry.value}`)
        .catch(() => {
          this.loading = false;
          this.countryError = true;
          setTimeout(() => {
            this.countryError = false;
          }, 5000);
          this.countriesUpdate.confirmed = 0;
          this.countriesUpdate.deaths = 0;
          this.countriesUpdate.recovered = 0;
          this.countriesUpdate.lastUpdate = "";
        });
      this.loading = false;
      this.countriesUpdate.confirmed = data.confirmed.value;
      this.countriesUpdate.deaths = data.deaths.value;
      this.countriesUpdate.recovered = data.recovered.value;
      this.countriesUpdate.lastUpdate = moment(data.lastUpdate).format(
        "MMMM Do YYYY, h:mm:ss a"
      );
    }
  },
  filters: {
    putComma(number) {
      return Number(number).toLocaleString();
    }
  },
  created() {
    this.getRootData();
    this.getCountries();
    this.locatedCountry();
  }
};
</script>
<style  scoped>
.tiya {
  text-decoration: none;
  color: rgb(76, 175, 80) !important;
}
.v-sheet {
  border-radius: 10px;
  border: none;
}
.v-card {
  box-shadow: none;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 500;
}
</style>