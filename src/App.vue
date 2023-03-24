<template>
  <div class="container-sm wrapper">
    <div class="background">
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
    </div>

    <h1 class="h1">ZIP Code Lookup</h1>

    <form @submit.prevent="searchZipCode">
      <label for="zip" class="h4">ZIP Code:</label>

      <input
        type="text"
        id="zip"
        v-model="zipCode"
        class="form-control"
        placeholder="Write your ZIP code"
        style="margin-bottom: 10px"
        @keydown.enter="searchZipCode"
      />

      <div v-if="errorMessage">
        <p class="h4 text-danger">{{ errorMessage }}</p>
      </div>

      <div class="d-flex justify-content-center">
        <button
          type="submit"
          class="btn btn-outline-secondary"
          style="width: 600px"
        >
          Show ZIP code info
        </button>
      </div>
    </form>

    <div v-if="showResults">
      <p class="h6"><strong>State:</strong> {{ state }}</p>

      <p class="h6"><strong>City:</strong> {{ city }}</p>
    </div>

    <div class="d-flex justify-content-center">
      <button
        @click="clear"
        class="btn btn-outline-secondary"
        style="width: 600px"
      >
        Clear result
      </button>
    </div>

    <div class="d-flex justify-content-center">
      <button
        v-if="showResults"
        @click="getIPInfo"
        class="btn btn-outline-secondary"
        style="width: 600px"
      >
        IP Lookup
      </button>
    </div>

    <div v-if="ipInfo">
      <p class="h6"><strong>ISP:</strong> {{ ipInfo.org }}</p>

      <p class="h6"><strong>City:</strong> {{ ipInfo.city }}</p>

      <p class="h6"><strong>Region:</strong> {{ ipInfo.region }}</p>

      <p class="h6"><strong>Country:</strong> {{ ipInfo.country }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      zipCode: "",
      state: "",
      city: "",
      showResults: false,
      ipInfo: null,
      errorMessage: "",
    };
  },
  methods: {
    searchZipCode() {
      axios
        .get(`https://api.zippopotam.us/us/${this.zipCode}`)
        .then((response) => {
          const data = response.data;
          if (data.places.length === 0) {
            this.errorMessage = "Invalid ZIP code";
            this.showResults = false;
            return;
          }
          this.state = data.places[0].state;
          this.city = data.places[0]["place name"];
          this.showResults = true;
          this.errorMessage = "";
        })
        .catch((error) => {
          console.log(error);
          this.errorMessage = "No such ZIP code, try another one";
          this.showResults = false;
        });
    },
    getIPInfo() {
      axios
        .get("https://ipapi.co/json/")
        .then((response) => {
          this.ipInfo = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    clear() {
      this.zipCode = "";
      this.state = "";
      this.city = "";
      this.showResults = false;
      this.ipInfo = null;
      this.errorMessage = "";
    },
  },
};
</script>
