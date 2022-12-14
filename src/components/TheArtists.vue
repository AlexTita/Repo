<script>
export default {
  data() {
    return {
      allArtists: null,
      artists: [],
    };
  },
  methods: {
    fetchArtists() {
      const apiKey = "5732de1113ade3d82c6082664921fbdc";
      const Url = `https://ws.audioscrobbler.com/2.0/?method=chart.gettopartists&api_key=${apiKey}&format=json`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.allArtists = data;
        });
    },
    showArtist() {
      this.artists = this.allArtists.artists.artist;
    },
  },
};
</script>

<template>
  <v-btn @click="fetchArtists">Get my Artists</v-btn>
  <div v-if="allArtists">
    <v-btn @click="showArtist"> Show Me The Artists </v-btn>
    <v-card
      v-for="(artist, index) in artists"
      :key="index"
      max-width="374"
      class="mx-auto my-12"
    >
      <v-img :src="artist.image[3]['#text']"> </v-img>
      <v-btn variant="plain" color="error" :href="artist.url" exact="true">
        {{ artist.name }}
      </v-btn>
      <v-card-text>
        <div>Playcount: {{ artist.playcount }}</div>
        <div>Monthly listeners: {{ artist.listeners }}</div>
      </v-card-text>
    </v-card>
  </div>
</template>

<style scoped>
img {
  float: left;
  margin-right: -165px;
}
</style>
