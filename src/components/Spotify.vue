<template>
    <div class="w-full h-full bg-cover bg-no-repeat gap-1  bg-center bg-[url(@/assets/spotify-background.jpg)] flex justify-center items-center">
        <div class="w-[25vw] h-full   gap-2 flex flex-col items-center justify-center">
            <div class="w-[20vw] h-[5vh]  flex items-center spotifyfont text-[#1DB954]">Spotify Informations - {{ this.$store.state.userData.display_name }}</div>
            <div class="w-[20vw] h-[30vh]  flex flex-col " >
                <div class="w-[12.5vw] h-[20vh]  rounded-md " :style="GetUserProfile()"></div>
            </div>
            <div class="w-[20vw] h-[50vh] flex flex-col ">
                <div class="w-[20vw] h-[5vh]  flex justify-between items-center">
                    <p class="spotifyfont text-white ">Product Type</p>
                    <p class="spotifyfont  text-[#1DB954] capitalize">{{ this.$store.state.userData.product }}</p>
                </div>
                <div class="w-[20vw] h-[5vh]  flex justify-between items-center">
                    <p class="spotifyfont text-white ">Total Friends</p>
                    <p class="spotifyfont  text-[#1DB954] capitalize">{{ this.$store.state.userData.followers.total }}</p>
                </div>
                <div class="w-[20vw] h-[5vh]  flex justify-between items-center">
                    <p class="spotifyfont text-white ">Country</p>
                    <p class="spotifyfont  text-[#1DB954] capitalize">{{ this.$store.state.userData.country }}</p>
                </div>
                <div class="w-[20vw] h-[5vh]  flex justify-between items-center">
                    <a class="spotifyfont  text-[#1DB954] capitalize" target="_blank" :href="this.$store.state.userData.external_urls.spotify">Your Spotify Link</a>
                </div>
            </div>
        </div>
        <div class="w-[50vw] h-full flex  justify-center items-center text-gray text-xs" style="text-indent : 30px;">
            <p>Discover the world of music like never before with Spotify, the leading music and audio content platform that provides access to millions of songs and podcasts from around the globe. Spotify is revolutionizing the way we enjoy music, making it easier than ever to find the perfect track or dive into interesting podcasts. Whether you're into the latest hits, classical compositions, or niche indie tunes, Spotify offers a vast library to cater to your musical preferences.
One of the most remarkable features of Spotify is its personalization. The platform offers tailored recommendations, daily mixes, and artist radios to enhance your listening experience. Explore playlists curated to match your mood, activity, or time of day, and stay up-to-date with the latest releases from your favorite artists. With Spotify, the music is always in sync with your life.
By integrating Spotify on your website, you can bring this incredible world of music to your visitors. Share your favorite playlists, collaborate on shared playlists with friends, and create a musical atmosphere that resonates with your content. However, it's essential to keep in mind that Spotify's content may be subject to copyright, and it's crucial to adhere to copyright rules and regulations while using it.

This website was created by developer.spotify.com.

</p>
        </div>
        <div class="w-[25vw] h-full   gap-2 flex flex-col items-center justify-center">
            <div class="w-[20vw] h-[5vh]   flex items-center justify-end spotifyfont text-[#1DB954]">Your Top List</div>
            <swiper class="w-[20vw] h-[80vh]   flex flex-col justify-end items-center" 
            :modules="modules"
            :slides-per-view="1"
            
            
            :pagination="{ clickable: true }"


            >
                <swiper-slide v-for="(item, index) in artists" class="  w-full h-full flex flex-col items-center justify-center ">
                    <div class="w-full h-full flex justify-center items-center flex flex-col gap-[1vh]">
                        <h1 class="spotifyfont text-[#1DB954]">#{{ index+1 }}</h1>
                        <div class="w-[15vw]  h-[20vh]  rounded-md " :style="{'background-image': 'url(' + item.images[1].url + ')','background-size': 'cover','background-repeat': 'no-repeat','background-position': 'center'}"></div>
                        <div class="w-[15vw] h-[5vh]  flex justify-center items-center spotifyfont text-[#1DB954]">{{ item.name }}</div>
                        <div class="w-[15vw] h-[30vh]  flex flex-col gap-2">
                            <div class="w-full h-[9vh]  flex flex-col justify-between items-center ">
                                <p class="spotifyfont text-white ">Popularity</p>
                                <div class="w-full bg-[#1db95441] h-[.8vh] rounded-md overflow-hidden">
                                    <div class="h-full bg-[#1DB954] rounded-md" :style="{ width: item.popularity + '%' }"></div>
                                </div>
                                <p class="spotifyfont text-white text-l">{{ item.popularity }}%</p>
                            </div>
                            <div class="w-full h-[9vh]  flex flex-col justify-between items-center ">
                                <p class="spotifyfont text-white ">Genres</p>
                                <div class="w-full h-[4vh] spotifyfont text-m ">
                                    <p v-for="(item, index) in item.genres" class="spotifyfont text-[#1DB954] text-l capitalize">{{ item }}</p>
                                </div>
                                
                            </div>
                            
                        </div>
                        <div class="w-[15vw] h-[14vh]  text-s text-white spotifyfont flex justify-center items-center flex-col gap-3">
                            <p>{{item.toptrackname}}</p>
                            <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 48 48" id="spotify"><g @click="PlayOrStop(item.toptrackid)" fill="none" fill-rule="evenodd"><g fill="black" class="cursor-pointer hover:fill-[#00DA5A] transition-all duration-300 " transform="translate(-200 -460)"><path d="M238.16 481.36c-7.68-4.56-20.52-5.04-27.84-2.76-1.2.36-2.4-.36-2.76-1.44-.36-1.2.36-2.4 1.44-2.76 8.52-2.52 22.56-2.04 31.44 3.24 1.08.6 1.44 2.04.84 3.12-.6.84-2.04 1.2-3.12.6m-.24 6.72c-.6.84-1.68 1.2-2.52.6-6.48-3.96-16.32-5.16-23.88-2.76-.96.24-2.04-.24-2.28-1.2-.24-.96.24-2.04 1.2-2.28 8.76-2.64 19.56-1.32 27 3.24.72.36 1.08 1.56.48 2.4m-2.88 6.6c-.48.72-1.32.96-2.04.48-5.64-3.48-12.72-4.2-21.12-2.28-.84.24-1.56-.36-1.8-1.08-.24-.84.36-1.56 1.08-1.8 9.12-2.04 17.04-1.2 23.28 2.64.84.36.96 1.32.6 2.04M224 460c-13.2 0-24 10.8-24 24s10.8 24 24 24 24-10.8 24-24-10.68-24-24-24"></path></g></g></svg>
                            
                        </div>
                    </div>

                </swiper-slide>
            </swiper>
        </div>
    </div>
</template>
  <!-- :autoplay="{ delay: 1500 }" -->
  <script>
   import { Swiper, SwiperSlide } from 'swiper/vue';
   import { Navigation, Pagination, Scrollbar, A11y, Autoplay  } from 'swiper/modules';
// Import Swiper styles
import 'swiper/css';
import 'swiper/css/navigation';
import 'swiper/css/pagination';
import 'swiper/css/scrollbar';
import 'swiper/css/autoplay';
  export default {
    name: 'Spotify',
    components: {
        Swiper,
      SwiperSlide,
    },
    setup() {
      const onSwiper = (swiper) => {
        console.log(swiper);
      };
      const onSlideChange = () => {
        console.log('slide change');
      };
      return {
        onSwiper,
        onSlideChange,
        modules: [Navigation, Pagination, Scrollbar, A11y, Autoplay],
      };
    },

    data() {
      return {
        artists : {}
      };
    },
    mounted() {
        this.getUserTopArtists()
        
    },
    methods: {
        async PlayOrStop(albumid) {
            console.log("heehehehhe", albumid)
            const url = "https://api.spotify.com/v1/me/player/play";
            const token = this.$store.state.token; // Spotify API anahtarınızı buraya ekleyin
                    
            const headers = {
              Authorization: `Bearer ${token}`,
              "Content-Type": "application/json",
            // Diğer gerekli başlıkları buraya ekleyebilirsiniz
            };
        
            const data = {
              context_uri: albumid,
              offset: {
                position: 1,
              },
              position_ms: 60,
            };
        
            try {
              const response = await fetch(url, {
                method: "PUT",
                headers: headers,
                body: JSON.stringify(data),
              });
          
              if (!response.ok) {
                throw new Error(`HTTP error! Status: ${response.status}`);
              }
          
              // Başarıyla gerçekleşen işlem
              console.log("Müziği oynatma isteği başarıyla gönderildi.");
            } catch (error) {
              console.error("Hata:", error);
            }
        },
        GetUserProfile() {
            return {
                'background-image': 'url(' + this.$store.state.userData.images[1].url + ')',
                'background-size': 'cover',
                'background-repeat': 'no-repeat',
                'background-position': 'center'
            }
            
        
    }
        },
        async getUserTopArtists() {
            try {
              const response = await fetch('https://api.spotify.com/v1/me/top/artists', {
                method: "GET",
                headers: {
                  Authorization: `Bearer ${this.$store.state.token}`, // Spotify API anahtarınızı buraya ekleyin
                },
              });
          
              if (!response.ok) {
                throw new Error(`HTTP error! Status: ${response.status}`);
              }
          
              const data = await response.json();
              
              this.artists = data.items;
            } catch (error) {
              console.error("Hata:", error);
            }
            for (let index = 0; index < this.artists.length; index++) {
                try {
              const response = await fetch(`https://api.spotify.com/v1/artists/${this.artists[index].id}/top-tracks?market=ES`, {
                method: "GET",
                headers: {
                  Authorization: `Bearer ${this.$store.state.token}`, // Spotify API anahtarınızı buraya ekleyin
                },
              });
          
          
              const data = await response.json();
              this.artists[index].toptrackid = data.tracks[0].album.uri
              this.artists[index].toptrackname = data.tracks[0].album.name
            } catch (error) {
              console.error("Hata:", error);
            }
            }

    },


  };
  </script>
  
  <style scoped>

  </style>
  