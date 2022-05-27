<template>
  <div id="app" class="app-container">
    <div class="app-header">
      <div class="app-header-left">
        <p class="app-name">Punk API</p>
        <SearchBar :search.sync="search" :beersSortType.sync="beersSortType" />
      </div>
      <div class="app-header-right">
        <button class="mode-switch" title="Switch Theme" @click="darkmodeToggle">
          <svg
            class="moon"
            fill="none"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            width="24"
            height="24"
            viewBox="0 0 24 24"
          >
            <defs></defs>
            <path d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"></path>
          </svg>
        </button>
        <button class="action-button active" @click="openlistFavoriteBeers">
          <svg
            viewBox="0 0 24 24"
            width="24"
            height="24"
            stroke="currentColor"
            stroke-width="2"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="css-i6dzq1"
          >
            <path
              d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
            ></path>
          </svg>
        </button>
      </div>
    </div>
    <div class="app-content">
      <div class="projects-section">
        <div class="projects-section-header">
          <p>{{ beers.length }} Beers</p>

          <div class="projects-section-line">
            <div class="view-actions">
              <button
                class="view-btn grid-view active"
                title="Grid View"
                @click="loadMore"
                v-if="currentPage * maxPerPage < beers.length"
              >
                <svg
                  viewBox="0 0 24 24"
                  width="24"
                  height="24"
                  stroke="currentColor"
                  stroke-width="2"
                  fill="none"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  class="css-i6dzq1"
                >
                  <rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect>
                  <line x1="12" y1="8" x2="12" y2="16"></line>
                  <line x1="8" y1="12" x2="16" y2="12"></line>
                </svg>
              </button>
            </div>
          </div>
        </div>
        <div class="project-boxes jsGridView">
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
              :beersArray="beers"
              :favoritesArray_prop.sync="favoritesArray"
              
            />
          </div>
        </div>
      </div>
    </div>

    <div class="app-right">
      <button class="checkout-button">
        <svg
          viewBox="0 0 24 24"
          width="20"
          height="20"
          stroke="currentColor"
          stroke-width="1.5"
          fill="white"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="css-i6dzq1"
        >
          <path
            d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
          ></path></svg
        >&nbsp; Your favorite beers
      </button>
      <button class="app-right-hide" @click="closelistFavoriteBeers">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="feather feather-x"
          viewBox="0 0 24 24"
        >
          <path d="M18 6L6 18M6 6l12 12" />
        </svg>
      </button>
      <div class="app-right-content">
        <div class="beer">
          <BeerCard
            class="beer_card"
            v-for="beer in favoritesArray"
            :key="beer.id"
            :name="beer.name"
            :img="beer.img"
            :abv="beer.abv"
            :tagline="beer.tagline"
            :desc="beer.desc"
            :tips="beer.tips"
            :food="beer.food"
            :beersArray="beers"
            :favoritesArray_prop.sync="favoritesArray"
          />
        </div>
      </div>
    </div>
    <FooterBar class="footer" text="@ Léo Genot" />
  </div>
</template>
<style>
@import url("../src/assets/App.css");
</style>
<script>
const listItems = document.querySelectorAll(".fadeIn");
const productBoxes = document.querySelectorAll(".product-box");

listItems.forEach(function (listItem, index) {
  listItem.setAttribute("style", `animation-delay: ${index * 0.2}s`);
});

productBoxes.forEach(function (productBox, index) {
  productBox.setAttribute("style", `animation-delay: ${index * 0.1}s`);
});

import BeerCard from "./components/BeerCard.vue";
import SearchBar from "./components/SearchBar.vue";
import FooterBar from "./components/FooterBar.vue";
import axios from "axios";

export default {
  name: "BeerGallery",
  components: {
    BeerCard,
    SearchBar,
    FooterBar,
  },
  data() {
    return {
      bottom: false,
      beers: [],
      favoritesArray: [],
      favoriteBeers: JSON.parse(localStorage.getItem("favorites")) || [],
      search: localStorage.getItem("search") || "",
      beersSortType: localStorage.getItem("beersSortType") || "AZName",
      currentPage: 1,
      maxPerPage: 3,
      showReadMore: true,
    };
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
  },
  methods: {


    loadMore() {
      this.currentPage += 1;
      this.addMoreBeer(this.currentPage);
    },
    addBeer() {
      //ONLY KEEP CLEAR FOR DEV PURPOSES DO NOT FORGET TO REMOVE IT AFTER
      localStorage.clear();
      axios
        .get("https://api.punkapi.com/v2/beers?page=1&per_page=30")
        .then((response) => {
          for (let i = 0; i < 30; i++) {
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
        .get("https://api.punkapi.com/v2/beers?page=" + page + "&per_page=30")
        .then((response) => {
          for (let i = 0; i < 30; i++) {
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
    darkmodeToggle() {
      var modeSwitch = document.querySelector(".mode-switch");

      document.documentElement.classList.toggle("dark");
      modeSwitch.classList.toggle("active");

      console.log(this.favoritesArray)
    },
    openlistFavoriteBeers() {
      document.querySelector(".app-right").classList.add("isOpen");
    },
    closelistFavoriteBeers() {
      document.querySelector(".app-right").classList.remove("isOpen");
    },


  },
};
</script>
