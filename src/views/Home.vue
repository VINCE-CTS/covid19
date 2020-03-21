<template>
  <div>
    <div v-if="loading">loading...</div>
    <!-- <div v-if="background">
      <img :src="background" alt="covid background image" />
    </div> -->
    <div style="display: flex;">
      <div style="margin: 2rem;">Confirmed: {{ confirmed }}</div>
      <div style="margin: 2rem;">Recovered: {{ recovered }}</div>
      <div style="margin: 2rem;">Death: {{ deaths }}</div>
    </div>
    <div>
      <h3>Cas par pays</h3>
      <div v-if="confirmedByCountry.length">
        <div v-for="(cas, index) in confirmedByCountry" :key="index">
          <span style="margin-left: 1rem;">{{ cas.countryRegion }}</span>
          <span style="margin-left: 1rem;">{{ cas.confirmed }}</span>
          <span style="background-color: green; margin-left: 1rem;">{{
            cas.recovered
          }}</span>
          <span style="background-color: red; margin-left: 1rem;">{{
            cas.deaths
          }}</span>
          <span style="background-color: orange; margin-left: 1rem;">{{
            cas.active
          }}</span>
          <span style="margin-left: 1rem;">{{
            new Date(cas.lastUpdate).toISOString().substr(0, 10)
          }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const BASE_URL = "https://covid19.mathdro.id/api";
export default {
  name: "Home",
  data() {
    return {
      loading: false,
      loadConfirmed: false,
      confirmed: null,
      confirmedByCountry: [],
      recovered: null,
      deaths: null,
      background: ""
    };
  },
  mounted() {
    axios({
      url: BASE_URL,
      method: "GET",
      headers: {
        "content-type": "application/json"
      }
    })
      .then(response => {
        console.log(response.data);
        this.confirmed = response.data.confirmed.value;
        this.recovered = response.data.recovered.value;
        this.deaths = response.data.deaths.value;
        this.background = response.data.image;
      })
      .catch(err => {
        console.error(err.message);
      })
      .then(() => {
        this.loading = false;
      });

    this.getAllConfirmedByCountry();
  },
  methods: {
    getAllConfirmedByCountry() {
      this.loadConfirmed = true;
      axios({
        url: `${BASE_URL}/confirmed`,
        method: "GET",
        headers: {
          "content-type": "application/json"
        }
      })
        .then(response => {
          console.log(response.data);
          this.confirmedByCountry = response.data;
        })
        .catch(err => {
          console.error(err);
        })
        .then(() => {
          this.loadConfirmed = false;
        });
    }
  }
};
</script>
