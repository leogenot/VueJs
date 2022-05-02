<template>
  <div id="app">
    <section>
      <div class="list-options">
        <input type="text" v-model="search" placeholder="Type a beer name" />
        <label for="beer-sort">Sort by : </label>
        <select v-model="beersSortType" id="beer-sort">
          <option value="AZName">Sort from A to Z</option>
          <option value="ZAName">Sort from Z to A</option>
          <option value="PlusABV">Sort from least alcohol</option>
          <option value="MinusABV">Sort from most alcohol</option>
        </select>
        <h1>{{ beers.length }} Beers</h1>
        <!-- <favorite name="favorite"></favorite> -->
      </div>
      <div v-if="!beers.length" class="loading">Loading...</div>
      <div class="beer">
        <BeerCard
          class="beer_card"
          v-for="beer in beersOrganizationData"
          :key="beer.id"
          :name="beer.name"
          :img="beer.img"
          :abv="beer.abv"
          :tagline="beer.tagline"
          :desc="beer.desc"
          :tips="beer.tips"
          :food="beer.food"
        />
      </div>
    </section>
    <button @click="loadMore" v-if="currentPage * maxPerPage < beers.length">
      load more
    </button>
  </div>
</template>
<style>
.beer {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));

  grid-auto-rows: auto;
  grid-gap: 1rem;
  position: relative;
  min-height: 100vh;
}
</style>
<script>
import BeerCard from "./components/BeerCard.vue";
//import PaginationComp from './components/PaginationComp.vue'
//import Favorite from "./components/favorite.vue";
import axios from "axios";


export default {
  name: "BeerGallery",
  components: {
    BeerCard,
    //PaginationComp,
    //Favorite,
  },
  data() {
    return {
      bottom: false,
      beers: [],
      search: "",
      beersSortType: "AZName",
      currentPage: 1,
      maxPerPage: 3,
      showReadMore: true,
    };
  },
  watch: {
  },
  created() {
    this.addBeer();
  },
  computed: {
    beersOrganizationData() {
      const field = ["AZName", "ZAName"].includes(this.beersSortType)
        ? "name"
        : "abv";
      const reversed = ["ZAName", "MinusABV"].includes(this.beersSortType);
      const filterFunc = (a) =>
        a.name.toLowerCase().includes(this.search.toLowerCase());
      let comparator = undefined;
      if (field == "name") {
        comparator = (a, b) => a.name.localeCompare(b.name);
      } else if (field == "abv") {
        comparator = (a, b) => parseFloat(a.abv) - parseFloat(b.abv);
      }

      let data = this.beers.filter(filterFunc);
      data = data.sort(comparator);
      if (reversed) data = data.reverse();
      return data;
    },
    totalResults() {
      return Object.keys(this.beers).length;
    },
    pageCount() {
      return Math.ceil(this.totalResults / this.maxPerPage);
    },
    pageOffest() {
      return this.maxPerPage * this.currentPage;
    },
    paginatedBeers() {
      return this.beers.slice(0, this.currentPage * this.maxPerPage);
    },
  },
  methods: {

    loadMore() {
      this.currentPage += 1;
      this.addMoreBeer(this.currentPage);
    },
    addBeer() {
      axios
        .get('https://api.punkapi.com/v2/beers?page=1&per_page=30')
        .then((response) => {
          for (let i = 0; i < 80; i++) {
            let api = response.data[i];
            let apiInfo = {
              name: api.name,
              abv: api.abv,
              desc: api.description,
              img: api.image_url,
              tips: api.brewers_tips,
              tagline: api.tagline,
              food: api.food_pairing,
            };
            this.beers.push(apiInfo);
          }

        });
    },
    addMoreBeer(page) {
      axios
        .get('https://api.punkapi.com/v2/beers?page=' + page + '&per_page=30')
        .then((response) => {
          for (let i = 0; i < 80; i++) {
            let api = response.data[i];
            let apiInfo = {
              name: api.name,
              abv: api.abv,
              desc: api.description,
              img: api.image_url,
              tips: api.brewers_tips,
              tagline: api.tagline,
              food: api.food_pairing,
            };
            this.beers.push(apiInfo);
          }

        });
    },
    
  },
};
</script>
