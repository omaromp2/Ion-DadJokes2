<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>DadJokes</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">DadJokes</ion-title>
        </ion-toolbar>
      </ion-header>

      <div id="container">
        
        <swiper 
        :modules="[Virtual]" 
        :slides-per-view="1" 
        :space-between="50" 
        @swiper="pushJoke" 
        @reachEnd="pushJoke" 
        virtual >
          <swiper-slide v-for="joke in jokes" :key="joke.id" >
            <ion-card>
              <ion-card-header>
                <ion-card-title>
                  <h1> <ion-icon :icon="glassesOutline" class="bold" ></ion-icon> </h1>
                  </ion-card-title>
                  <ion-card-subtitle>
                    
                  </ion-card-subtitle>
              </ion-card-header>

              <ion-card-content>
                {{ joke.joke }}
                <br>
                <ion-button @click="shareJoke(joke.joke)" >
                  share
                </ion-button>

              </ion-card-content>
            </ion-card>
          </swiper-slide>
        </swiper>

      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="js">
  import {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonCard, 
    IonCardContent, 
    IonCardHeader, 
    IonCardSubtitle, 
    IonCardTitle, 
    IonIcon, 
    IonButton,
    // IonSlides,
    // IonSlide
  } from '@ionic/vue';

  import { glassesOutline } from "ionicons/icons";

  import {
    defineComponent
  } from 'vue';

  import axios from 'axios';

  // Import Swiper Vue.js components
  import { 
    Swiper,
    SwiperSlide
  } from 'swiper/vue';

  import { Virtual } from 'swiper';

  // Import Swiper styles
  import 'swiper/css';

  import { Share } from '@capacitor/share';


  export default defineComponent({
    name: 'HomePage',
    components: {
      IonContent,
      IonHeader,
      IonPage,
      IonTitle,
      IonToolbar,
      Swiper,
      SwiperSlide, 
      IonCard, IonCardContent, IonCardHeader,
      IonCardSubtitle, 
      IonCardTitle, 
      IonIcon, 
      IonButton
    },
    data() {
      return {
        // icons
        glassesOutline,
        // Vars
        jokes: []
      }
    },

    methods:{

       pushJoke(){
          axios.get('https://icanhazdadjoke.com/', {
            headers: {
              'accept': 'application/json'
            }
          })
          .then(resp => {
            console.log(resp.data);
            // this.joke = resp.data.joke;
            // const joke = JSON.parse(resp.data); 
            this.jokes.push(resp.data);
            // this.jokes = resp.data; 
          })
          .catch(err => {
            console.log(err);
          });
      },

      async shareJoke(joke) {
        await Share.share({
          title: 'Share Joke', 
          text: joke, 
          dialogTitle: "Share with buddies",
        });
      }

    },
    
    setup() {

      const onSwiper = (swiper) => {
        console.log(swiper);
        // pushJoke(); 
        this.hello();
      };
      const onSlideChange = () => {
        console.log('slide change');
        // this.pushJoke();
        // this.hello();
      };
      return {
        onSwiper,
        onSlideChange,
        // pushJoke,
        Virtual, 
      };
    }

  });
</script>

<style scoped>
  #container {
    text-align: center;

    position: absolute;
    left: 0;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
  }

  #container strong {
    font-size: 20px;
    line-height: 26px;
  }

  #container p {
    font-size: 16px;
    line-height: 22px;

    color: #8c8c8c;

    margin: 0;
  }

  #container a {
    text-decoration: none;
  }
</style>