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
    
    <h1 class="display-1">ZIP Code Lookup</h1>

    <form>
      <label for="zip" class="display-6">ZIP Code:</label>

      <input type="text" id="zip" v-model="zipCode" class="form-control" placeholder="Write your ZIP code">
    </form>

      <button type="submit" @click.prevent="searchZipCode" class="btn btn-outline-secondary">Render</button>

    <div v-if="showResults">
      <p class="display-6"><strong>State:</strong> {{ state }}</p>

      <p class="display-6"><strong>City:</strong> {{ city }}</p>
    </div>

    <button @click="goToHome" class="btn btn-outline-secondary">Main Page</button>

    <button v-if="showResults" @click="getIPInfo" class="btn btn-outline-secondary">IP Lookup</button>

    <div v-if="ipInfo">
      <p class="display-6"><strong>ISP:</strong> {{ ipInfo.org }}</p>

      <p class="display-6"><strong>City:</strong> {{ ipInfo.city }}</p>

      <p class="display-6"><strong>Region:</strong> {{ ipInfo.region }}</p>

      <p class="display-6"><strong>Country:</strong> {{ ipInfo.country }}</p>
    </div>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      zipCode: '',
      state: '',
      city: '',
      showResults: false,
      ipInfo: null,
    };
  },
  methods: {
    searchZipCode() {
      axios.get(`https://api.zippopotam.us/us/${this.zipCode}`)
        .then(response => {
          const data = response.data;
          this.state = data.places[0].state;
          this.city = data.places[0]["place name"];
          this.showResults = true;
        })
        .catch(error => {
          console.log(error);
        });
    },
    getIPInfo() {
      axios.get('https://ipapi.co/json/')
        .then(response => {
          this.ipInfo = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    goToHome() {
      window.location.href = '/ZipCode-VueJs/';
    },
  },
};
</script>
