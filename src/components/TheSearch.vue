<script>
export default {
  data() {
    return {
      text: "",
      leradio: "artist",
      whatever: null,
      artists: null,
      tracks: null,
      albums: null,
    };
  },
  methods: {
    fetchStuff() {
      const apiKey = "5732de1113ade3d82c6082664921fbdc";
      const Url1 = `http://ws.audioscrobbler.com/2.0/?method=artist.search&artist=${this.text}&api_key=${apiKey}&format=json`;
      const Url2 = `http://ws.audioscrobbler.com/2.0/?method=track.search&track=${this.text}&api_key=${apiKey}&format=json`;
      const Url3 = `http://ws.audioscrobbler.com/2.0/?method=album.search&album=${this.text}&api_key=${apiKey}&format=json`;
      if (this.leradio == "artist") {
        fetch(Url1)
          .then((response) => response.json())
          .then((data) => {
            this.whatever = data;
            this.artists = this.whatever.results.artistmatches.artist;
            this.tracks = null;
            this.albums = null;
          });
      }
      if (this.leradio == "track") {
        fetch(Url2)
          .then((response) => response.json())
          .then((data) => {
            this.whatever = data;
            this.artists = null;
            this.tracks = this.whatever.results.trackmatches.track;
            this.albums = null;
          });
      }
      if (this.leradio == "album") {
        fetch(Url3)
          .then((response) => response.json())
          .then((data) => {
            this.whatever = data;
            this.artists = null;
            this.tracks = null;
            this.albums = this.whatever.results.albummatches.album;
          });
      }
    },
  },
};
</script>

<template>
  <div>
    <v-text-field
      type="text"
      v-model="text"
      placeholder="Text"
      @keyup.enter="fetchStuff"
      @click:append-inner="fetchStuff"
      append-inner-icon="mdi-magnify"
    />

    <br />
    <v-radio-group v-model="leradio">
      <v-row>
        <v-radio
          label="search artist"
          color="indigo-darken-3"
          value="artist"
        ></v-radio>
        <v-radio
          label="search track"
          color="indigo-darken-3"
          value="track"
        ></v-radio>
        <v-radio
          label="search album"
          color="indigo-darken-3"
          value="album"
        ></v-radio>
      </v-row>
    </v-radio-group>
  </div>

  <v-container v-if="artists">
    <v-row>
      <v-col v-for="(artist, index) in artists" :key="index" :cols="3">
        <v-card max-width="374" class="mx-auto my-12">
          <v-img :src="artist.image[3]['#text']"> </v-img>
          <v-btn variant="plain" color="error" :href="artist.url" exact="true">
            {{ artist.name }}
          </v-btn>
          <v-card-text>
            <div>Monthly listeners: {{ artist.listeners }}</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>

  <v-container v-if="tracks" fluid class="bg-surface-variant mb-6">
    <v-row>
      <v-col v-for="(track, index) in tracks" :key="index" :cols="3">
        <v-card max-width="374" class="mx-auto my-12">
          <v-img :src="track.image[3]['#text']"> </v-img>
          <v-btn variant="plain" color="error" :href="track.url" exact="true">
            {{ track.name }}
          </v-btn>
          {{ track.artist }}
          <v-card-text>
            <div>Monthly listeners: {{ track.listeners }}</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>

  <v-container v-if="albums" fluid class="bg-surface-variant mb-6">
    <v-row>
      <v-col v-for="(album, index) in albums" :key="index" :cols="3">
        <v-card max-width="374">
          <v-img :src="album.image[3]['#text']"> </v-img>
          <v-btn variant="plain" color="error" :href="album.url" exact="true">
            {{ album.name }}
          </v-btn>
          {{ album.artist }}
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
