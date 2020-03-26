<template>
  <div>
    <div class="container mx-auto">
      <div v-if="loading">loading...</div>

      <div class="flex justify-between my-4">
        <div
          class="bg-orange-500 w-14 rounded shadow mx-4 w-48 h-24 hover:bg-orange-400"
          v-if="confirmed"
        >
          <h4 class="text-center text-white mt-6">{{ confirmed }}</h4>
          <h6 class="text-center text-white">Confirmed</h6>
        </div>
        <div
          class="bg-green-500 rounded shadow mx-4 w-48 h-24 hover:bg-green-400"
          v-if="recovered"
        >
          <h4 class="text-center text-white mt-6">{{ recovered }}</h4>
          <h6 class="text-center text-white">Recovered</h6>
        </div>
        <div
          class="bg-red-500 rounded shadow mx-4 w-48 h-24 hover:bg-red-400"
          v-if="deaths"
        >
          <h4 class="text-center text-white mt-6">{{ deaths }}</h4>
          <h6 class="text-center text-white">Deaths</h6>
        </div>
      </div>

      <div class="flex">
        <div class="w-full lg:w-1/3 px-4 mt-2">
          <div class="bg-white border-t border-b sm:rounded sm:border shadow">
            <div class="border-b">
              <div class="flex justify-between px-6 -mb-px">
                <h3 class="text-blue-dark py-4 font-normal text-lg">
                  Confirmed
                </h3>
              </div>
            </div>
            <div>
              <div class="px-6 py-4">
                <div class="py-4">
                  <div v-if="confirmedByCountry.length">
                    <div
                      v-for="(cas, index) in confirmedByCountry"
                      :key="index"
                    >
                      <span class="mx-1 text-medium font-bold">{{
                        cas.provinceState
                          ? cas.provinceState
                          : cas.countryRegion
                      }}</span>
                      <span class="mx-1 bg-orange-500 rounded shadow">{{
                        cas.confirmed
                      }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="w-full lg:w-1/3 px-4 mt-2">
          <div class="bg-white border-t border-b sm:rounded sm:border shadow">
            <div class="border-b">
              <div class="flex justify-between px-6 -mb-px">
                <h3 class="text-blue-dark py-4 font-normal text-lg">
                  Recovered
                </h3>
              </div>
            </div>
            <div>
              <div class="px-6 py-4">
                <div class="py-4">
                  <div v-if="confirmedByCountry.length">
                    <div
                      v-for="(cas, index) in confirmedByCountry"
                      :key="index"
                    >
                      <span class="mx-1 text-medium font-bold">{{
                        cas.provinceState
                          ? cas.provinceState
                          : cas.countryRegion
                      }}</span>
                      <span class="mx-1 bg-green-500 rounded shadow">{{
                        cas.recovered
                      }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="w-full lg:w-1/3 px-4 mt-2">
          <div class="bg-white border-t border-b sm:rounded sm:border shadow">
            <div class="border-b">
              <div class="flex justify-between px-6 -mb-px">
                <h3 class="text-blue-dark py-4 font-normal text-lg">
                  Deaths
                </h3>
              </div>
            </div>
            <div>
              <div class="px-6 py-4">
                <div class="py-4">
                  <div v-if="confirmedByCountry.length">
                    <div
                      v-for="(cas, index) in confirmedByCountry"
                      :key="index"
                    >
                      <span class="mx-1 text-medium font-bold">{{
                        cas.provinceState
                          ? cas.provinceState
                          : cas.countryRegion
                      }}</span>
                      <span class="mx-1 bg-red-500 rounded shadow">{{
                        cas.deaths
                      }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="w-full lg:w-1/3 px-4 mt-2">
          <div class="bg-white border-t border-b sm:rounded sm:border shadow">
            <div class="border-b">
              <div class="flex justify-between px-6 -mb-px">
                <h3 class="text-blue-dark py-4 font-normal text-lg">
                  Actives
                </h3>
              </div>
            </div>
            <div>
              <div class="px-6 py-4">
                <div class="py-4">
                  <div v-if="confirmedByCountry.length">
                    <div
                      v-for="(cas, index) in confirmedByCountry"
                      :key="index"
                    >
                      <span class="mx-1 text-medium font-bold">{{
                        cas.provinceState
                          ? cas.provinceState
                          : cas.countryRegion
                      }}</span>
                      <span class="mx-1 bg-yellow-500 rounded shadow">{{
                        cas.active
                      }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
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
