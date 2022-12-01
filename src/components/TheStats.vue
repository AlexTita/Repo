<script>
export default {
  data() {
    return {
      allArtists: null,
    };
  },
  methods: {
    fetchArtists() {
      const apiKey = "5732de1113ade3d82c6082664921fbdc";
      const Url = `https://ws.audioscrobbler.com/2.0/?method=geo.gettopartists&country=romania&api_key=${apiKey}&format=json`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.allArtists = data;
        });
    },
  },
};
</script>

<template>
  <h2>DA</h2>
  <button @click="fetchArtists">Get my Artists</button>
</template>

<style scoped>
img {
  float: left;
  margin-right: -165px;
}
</style>
