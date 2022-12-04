<script>
import Home from "./Home.vue";
import Artists from "./Artists.vue";
import NotFound from "./404.vue";
import { useTheme } from "vuetify";
const routes = {
  "/": Home,
  "/artists": Artists,
};

export default {
  setup() {
    const theme = useTheme();

    return {
      theme,
      toggleTheme: () =>
        (theme.global.name.value = theme.global.current.value.dark
          ? "light"
          : "dark"),
    };
  },
  data() {
    return {
      currentPath: window.location.hash,
    };
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || "/"] || NotFound;
    },
  },
  mounted() {
    window.addEventListener("hashchange", () => {
      this.currentPath = window.location.hash;
    });
  },
  methods: {},
};
</script>

<template>
  <main>
    <main>
      <v-app>
        <v-app-bar title="Last Fm Statistics">
          <v-btn href="#/" icon="mdi-home"></v-btn>
          <v-btn href="#/artists" icon="mdi-account-music"></v-btn>
          <v-btn @click="toggleTheme" icon="mdi-invert-colors"></v-btn>
        </v-app-bar>
        <v-main>
          <component :is="currentView" />
        </v-main>
      </v-app>
    </main>
  </main>
</template>
