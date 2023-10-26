<template>
    <MainPage v-if="this.$store.state.page == 'main'"></MainPage>
    <Spotify v-if="this.$store.state.page == 'user-spotify'"></Spotify>
</template>

<script>

import MainPage from './components/MainPage.vue'
import Spotify from './components/Spotify.vue'
export default {
  name: 'App',
  components: {
      MainPage,
      Spotify
  }
}
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;

@font-face {
  font-family: "Netflix";
  src: url(./assets/fonts/netflix.ttf);
}
@font-face {
  font-family: "Spotify";
  src: url(./assets/fonts/spoti.otf);
}

#app {
  width: 100vw;
  height: 100vh;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* user-select: none; */
  overflow: hidden;
}

.netflixfont {
  font-family: Netflix;
}
.spotifyfont {
  font-family: Spotify;
}
</style>
