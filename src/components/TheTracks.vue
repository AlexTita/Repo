<script>
export default {
  data() {
    return {
      allTracks: null,
      tracks: [],
    };
  },
  methods: {
    fetchTracks() {
      const apiKey = "5732de1113ade3d82c6082664921fbdc";
      const Url = `https://ws.audioscrobbler.com/2.0/?method=chart.gettoptracks&api_key=${apiKey}&format=json`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.allTracks = data;
        });
    },
    showTracks() {
      this.tracks = this.allTracks.tracks.track;
    },
  },
};
</script>

<template>
  <v-btn @click="fetchTracks">Get my Songs</v-btn>
  <div v-if="allTracks">
    <v-btn @click="showTracks"> Show Me The Songs </v-btn>
    <v-card
      v-for="(track, index) in tracks"
      :key="index"
      max-width="374"
      class="mx-auto my-12"
    >
      <v-img :src="track.image[3]['#text']"> </v-img>
      <v-btn variant="plain" color="error" :href="track.url" exact="true">
        {{ track.name }}
      </v-btn>
      <v-btn
        variant="plain"
        color="error"
        :href="track.artist.url"
        exact="true"
      >
        {{ track.artist.name }}
      </v-btn>
      <v-card-text>
        <div>Playcount: {{ track.playcount }}</div>
        <div>Monthly listeners: {{ track.listeners }}</div>
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
