<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import '../node_modules/reset-css'
import './style/index.scss'
</script>

<template>
    <header class="container">
      <div class="header header-content">
        <a href="./index.html" class="header__logo">Top Films</a>
        <select name="" id="genre" class="header__genre action" v-model="simbol">
          <option value="1">Tриллер</option>
          <option value="2">Драма</option>
          <option value="3">Криминал</option>
          <option value="4">Мелодрама</option>
          <option value="5">Детектив</option>
          <option value="6">Фантастика</option>
          <option value="7">Приключения</option>
        </select>
        <span></span>
        <form>
          <input type="text" class="header__search" placeholder="Search" @keyup.enter="Serch"/>
        </form>
      </div>
    </header>
      <div class="container"> 
      <div class="movie__today"></div>
      <div class="movies">
        <!-- <div class="movie">
          <div class="movie__cover-inner">
            <img src="./img/thor.jpeg" alt="" class="movie__cover" />
            <div class="movie__cover--darkened"></div>
          </div>
          <div class="movie__info">
            <div class="movie__title">Thor: Love and Thunder</div>
            <div class="movie__category">Fantasy</div>
            <div class="movie__average movie__average--green">6.4</div>
          </div>
        </div> -->
      </div>
    </div>
    <div class="container" @click="Details" v-for="(item) in restaurants"> {{ simbol }}
      <div class="movie__today"></div>
      <div class="movies">
        <div class="movie"> {{restaurants.kinopoiskId}}
          <div class="movie__cover-inner">
            <img :src="restaurants[i].posterUrlPreview" alt="restaurants[i].nameRu" class="movie__cover" />
            <div class="movie__cover--darkened"></div>
          </div>
          <div class="movie__info">
            <div class="movie__title">Thor: Love and Thunder {{restaurants[i].nameRu}} </div>
            <div class="movie__category">Fantasy {{restaurants[i].genres}} </div>
            <div class="movie__average movie__average--green">6.4</div>
          </div>
        </div>
      </div>
    </div>

    <div id="myModal" class="modal">
      <span
        class="close"
        onclick="document.getElementById('myModal').style.display='none'"
        >&times;</span>
        <div class="modal__container">
      <img class="modal-content" id="img01" />
      <div class="modal__box">
      <h4 class="modal__title"></h4>
      <h5 class="modal__genre"></h5>
      <p class="modal__year"></p>
      <div class="modal__text"></div>
      <p class="modal__pg"></p></div>
    </div>
    </div>

    <button class="next__page" @click="popular">Show more</button>


   <RouterView />

 <div>
    <button @click="Serch">Serch</button>
    <button @click="Details">Details</button>
   </div>

</template>




<script>
export default {
  name: 'App',
  data () {
    return {
      out: '',
      simbol: '',
      restaurants: [],
      error: null,
      headers: {'Content-Type': 'application/json', "X-API-KEY": '8c8e1a50-6322-4135-8875-5d40a5420d86'},
    }
  },
  methods: {
    async popular() {
      await this.filter('https://kinopoiskapiunofficial.tech/api/v2.2/films/top?type=TOP_100_POPULAR_FILMS')
    },
    async Serch() {
      await this.filter('https://kinopoiskapiunofficial.tech/api/v2.1/films/search-by-keyword?keyword=')
    },
    async Details() {
      await this.filter('https://kinopoiskapiunofficial.tech/api/v2.2/films/')
    },
    async filter(api) {
      try {
        const response = await fetch(api, {
          method: 'GET',
          headers: this.headers,
        }).then(this.checkStatus)
            .then(this.parseJSON);
        console.log(response)
        this.restaurants = response
      } catch (error) {
        this.error = error
      }
    },
    parseJSON: function (resp) {
      return (resp.json ? resp.json() : resp);
    },
    checkStatus: function (resp) {
      if (resp.status >= 200 && resp.status < 300) {
        return resp;
      }
      return this.parseJSON(resp).then((resp) => {
        throw resp;
      });
    },
  
//     genre() {
// // console.log(this.simbol);
// // this.out = this.simbol
// switch (this.simbol) {
//   case '1':
//     this.out = 
//     this.restaurants.push(this.out)
//     break
//     case '2':
//     this.out = 
//     this.restaurants.push(this.out)
//     break
//     case '3':
//     this.out = 
//     this.restaurants.push(this.out)
//     break
// }
//  },
  },
  async mounted () {
    try {
      const response = await fetch('https://kinopoiskapiunofficial.tech/api/v2.2/films?genres=', {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
          .then(this.parseJSON);
      console.log(response)
      this.restaurants = response
    } catch (error) {
      this.error = error
    }
  },
}
</script>