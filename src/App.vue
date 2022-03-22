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
      </div>
      <div v-if="!beers.length" class="loading">Loading...</div>
      <BeerCard
        v-for="beer in beersOrganizationData"
        :key="beer"
        :name="beer.name"
        :img="beer.img"
        :abv="beer.abv"
        :tagline="beer.tagline"
        :desc="beer.desc"
        :tips="beer.tips"
        :food="beer.food"
      />
    </section>
  </div>
</template>

<script>
import BeerCard from "./components/BeerCard.vue";
import axios from "axios";
export default {
  name: "BeerGallery",
  components: {
    BeerCard,
  },
  data() {
    return {
      bottom: false,
      beers: [],
      search: "",
      beersSortType: "AZName",
    };
  },
  /* watch: {
    bottom(newValue) {
      if (newValue) {
        this.addBeer();
      }
    },
  }, */
  created() {
    /* window.addEventListener("scroll", () => {
      this.bottom = this.bottomVisible();
    }); */
    this.addBeer();
    this.addBeer();
    this.addBeer();
    this.addBeer();
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
      let comparator = undefined
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
    /*
    bottomVisible() {
      const scrollY = window.scrollY;
      const visible = document.documentElement.clientHeight;
      const pageHeight = document.documentElement.scrollHeight;
      const bottomOfPage = visible + scrollY >= pageHeight;
      return bottomOfPage || pageHeight < visible;
    }, */
    addBeer() {
      axios.get("https://api.punkapi.com/v2/beers/random").then((response) => {
        let api = response.data[0];
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
        /* if (this.bottomVisible()) {
          this.addBeer();
        } */
      });
    },
  },
};
</script>
