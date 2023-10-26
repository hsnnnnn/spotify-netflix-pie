<template>
  <div class="fullcontainer w-full h-full bg-[#191717] flex items-center gap-1 justify-center">
    <div class="w-[50vw] h-full bg-black flex items-center justify-center transition-all opacity-60 hover:opacity-80 flex-col bg-cover bg-no-repeat bg-center bg-[url(@/assets/netflixtv-2.jpg)]">
      <div class="w-[40vw] h-[10vh] flex justify-center items-center" style="margin-top: 25vh;">
        <p class="netflixfont text-3xl cursor-pointer text-white hover:text-4xl transition-all" @click="CallApi('netflix')">GET MY STATISTICS</p>
      </div>
    </div>
    <div class="w-[50vw] h-full bg-black flex items-center transition-all justify-center opacity-60 hover:opacity-80 flex-col bg-cover bg-no-repeat bg-center bg-[url(@/assets/spotifytv.jpg)]">
      <div class="w-[40vw] h-[10vh] flex justify-center items-center" style="margin-top: 25vh;">
        <p class="netflixfont text-3xl cursor-pointer text-white hover:text-4xl transition-all" @click="CallApi('spotify')">GET MY STATISTICS</p>
      </div>
    </div>
    <a class="absolute bottom-5 right-5 netflixfont" target="_blank" href="https://github.com/hsnnnnn">Created by hsn361</a>
  </div>
</template>

<script>
export default {
  name: 'MainPage',
  components: {},
  data() {
    return {
      clientId: "8ef5d1b0004849b48db5af3ea7259c49",
      code: null,
      params: new URLSearchParams(window.location.search),
    };
  },
  methods: {
    async CallApi(app) {
      if (app == 'spotify') {
        if (!this.code) {
          this.redirectToAuthCodeFlow(this.clientId);
        } else {
          const accessToken = await this.getAccessToken(this.clientId, this.code);
          const profile = await this.fetchProfile(accessToken);
          this.$store.state.token = accessToken
          this.$store.state.page = "user-spotify"
        }
      } else {
        this.$router.push('/netflix');
      }
    },
    async redirectToAuthCodeFlow(clientId) {
      const verifier = this.generateCodeVerifier(128);
      const challenge = await this.generateCodeChallenge(verifier);

      localStorage.setItem("verifier", verifier);

      const params = new URLSearchParams(window.location.search);
      params.append("client_id", clientId);
      params.append("response_type", "code");
      params.append("redirect_uri", "http://localhost:8080/"); // Doğru redirect URI'yi kullanın
      params.append("scope", "user-read-private user-read-email user-top-read user-modify-playback-state");
      params.append("code_challenge_method", "S256");
      params.append("code_challenge", challenge);
      
      document.location = `https://accounts.spotify.com/authorize?${params.toString()}`;
    },
    generateCodeVerifier(length) {
      let text = '';
      let possible = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';

      for (let i = 0; i < length; i++) {
        text += possible.charAt(Math.floor(Math.random() * possible.length));
      }
      return text;
    },
    async generateCodeChallenge(codeVerifier) {
      const data = new TextEncoder().encode(codeVerifier);
      const digest = await window.crypto.subtle.digest('SHA-256', data);
      return btoa(String.fromCharCode.apply(null, [...new Uint8Array(digest)]))
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=+$/, '');
    },
    async getAccessToken(clientId, code) {
      const verifier = localStorage.getItem("verifier");

      const params = new URLSearchParams(window.location.search);
      params.append("client_id", this.clientId);
      params.append("grant_type", "authorization_code");
      params.append("code", this.code);
      params.append("redirect_uri", "http://localhost:8080/"); // Doğru redirect URI'yi kullanın
      params.append("code_verifier", verifier);

      const result = await fetch("https://accounts.spotify.com/api/token", {
        method: "POST",
        headers: { "Content-Type": "application/x-www-form-urlencoded" },
        body: params,
      });

      const { access_token } = await result.json();
      return access_token;
    },
    async fetchProfile(token) {
      const result = await fetch("https://api.spotify.com/v1/me", {
        method: "GET",
        mode: 'cors',
        headers: { Authorization: `Bearer ${token}` },
      });

      return await result.json();
    },
  },
  async mounted() {
    if (this.params.has("code")) {
      this.code = this.params.get("code");
      const accessToken = await this.getAccessToken(this.clientId, this.code);
      const profile = await this.fetchProfile(accessToken);

      this.$store.state.userData = profile
      this.$store.state.token = accessToken
      this.$store.state.page = "user-spotify"
    }
  },
};
</script>

<style scoped>
</style>
