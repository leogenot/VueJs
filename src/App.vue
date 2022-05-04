<template>
  <div id="app" class="app-container">
    <div class="app-header">
      <div class="app-header-left">
        <p class="app-name">Punk API</p>
        <SearchBar :value="search" @input="search = $event" />
      </div>
      <div class="app-header-right">
        <button
          class="mode-switch"
          title="Switch Theme"
          @click="darkmodeToggle"
        >
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
              <div class="select-dropdown">
                <select v-model="beersSortType" id="beer-sort">
                  <option value="AZName">Sort from A to Z</option>
                  <option value="ZAName">Sort from Z to A</option>
                  <option value="PlusABV">Sort from least alcohol</option>
                  <option value="MinusABV">Sort from most alcohol</option>
                </select>
              </div>

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
            />
          </div>
        </div>
      </div>
    </div>

    <div class="app-right">
      <button class="checkout-button"> <svg
            viewBox="0 0 24 24"
            width="20"
            height="20"
            stroke="currentColor"
            stroke-width="1.5"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="css-i6dzq1"
          >
            <path
              d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
            ></path>
          </svg>&nbsp; Your favorite beers</button>
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
      </div>
      
    </div>
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
* {
  box-sizing: border-box;
}
:root {
  --app-container: #f3f6fd;
  --main-color: #1f1c2e;
  --secondary-color: #4a4a4a;
  --link-color: #1f1c2e;
  --link-color-hover: #c3cff4;
  --link-color-active: #fff;
  --link-color-active-bg: #1f1c2e;
  --projects-section: #fff;
  --message-box-hover: #fafcff;
  --message-box-border: #e9ebf0;
  --more-list-bg: #fff;
  --more-list-bg-hover: #f6fbff;
  --more-list-shadow: rgba(209, 209, 209, 0.4);
  --button-bg: #1f1c24;
  --search-area-bg: #fff;
  --star: #1ff1c2e;
  --message-btn: #fff;
}
body,
html {
  width: 100%;
  height: 100vh;
  margin: 0;
}
body {
  font-family: "DM Sans", sans-serif;
  overflow: hidden;
  display: flex;
  justify-content: center;
  background-color: var(--app-container);
}
a,
button {
  cursor: pointer;
}
#beer-sort {
  margin-right: 15px;
}
.select-dropdown {
  position: relative;
  background-color: var(--app-container);
  width: auto;
  float: left;
  max-width: 100%;
  border-radius: 2px;
}
.select-dropdown select {
  font-size: 1rem;
  font-weight: 200;
  max-width: 100%;
  padding: 8px 24px 8px 10px;
  border: none;
  background-color: transparent;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  color: var(--main-color);
}
.select-dropdown select:active,
.select-dropdown select:focus {
  outline: 0;
  box-shadow: none;
}
.select-dropdown:after {
  content: " ";
  position: absolute;
  top: 50%;
  margin-top: -2px;
  right: 8px;
  width: 0;
  height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 5px solid var(--main-color);
}
.app-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: var(--app-container);
  transition: 0.2s;
  max-width: 1800px;
}
.app-container button,
.app-container input,
.app-container select {
  font-family: "DM Sans", sans-serif;
}
.app-content {
  display: flex;
  height: 100%;
  overflow: hidden;
  padding: 16px 24px 24px 24px;
}
.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px 24px;
  position: relative;
}
.app-header-left,
.app-header-right {
  display: flex;
  align-items: center;
}
.app-header-left {
  flex-grow: 1;
}
.app-header-right button {
  margin-left: 10px;
}
.app-name {
  color: var(--main-color);
  margin: 0;
  font-size: 20px;
  line-height: 24px;
  font-weight: 700;
  margin: 0 32px;
}
.mode-switch {
  background-color: transparent;
  border: none;
  padding: 0;
  color: var(--main-color);
  display: flex;
  justify-content: center;
  align-items: center;
}
.search-wrapper {
  border-radius: 20px;
  background-color: var(--search-area-bg);
  padding-right: 12px;
  height: 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  max-width: 480px;
  color: var(--light-font);
  box-shadow: 0 2px 6px 0 rgba(136, 148, 171, 0.2),
    0 24px 20px -24px rgba(71, 82, 107, 0.1);
  overflow: hidden;
}
.search-input {
  border: none;
  flex: 1;
  outline: 0;
  height: 100%;
  padding: 0 20px;
  font-size: 16px;
  background-color: var(--search-area-bg);
  color: var(--main-color);
}
.search-input:placeholder {
  color: var(--main-color);
  opacity: 0.6;
}
.favorite-btn {
  color: var(--main-color);
  padding: 0;
  border: 0;
  background-color: transparent;
  height: 32px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.projects-section {
  flex: 2;
  background-color: var(--projects-section);
  border-radius: 32px;
  padding: 32px 32px 0 32px;
  overflow: hidden;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.projects-section-line {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 32px;
}
.projects-section-header {
  display: flex;
  justify-content: space-between;
  color: var(--main-color);
}
.projects-section-header p {
  font-size: 24px;
  line-height: 32px;
  font-weight: 700;
  opacity: 0.9;
  margin: 0;
  color: var(--main-color);
}
.view-actions {
  display: flex;
  align-items: center;
}
.view-btn {
  width: 36px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 6px;
  border-radius: 4px;
  background-color: transparent;
  border: none;
  color: var(--main-color);
  margin-left: 8px;
  transition: 0.2s;
}
.view-btn.active {
  background-color: var(--link-color-active-bg);
  color: var(--link-color-active);
}
.view-btn:not(.active):hover {
  background-color: var(--link-color-hover);
  color: var(--link-color-active);
}
.project-boxes {
  margin: 0 -8px;
  overflow-y: auto;
}
.mode-switch.active .moon {
  fill: var(--main-color);
}
@media screen and (max-width: 720px) {
  .app-name {
    display: none;
  }
  .favorite-btn,
  .mode-switch {
    width: 20px;
    height: 20px;
  }
  .favorite-btn svg,
  .mode-switch svg {
    width: 16px;
    height: 16px;
  }
  .app-header-right button {
    margin-left: 4px;
  }
}
@media screen and (max-width: 520px) {
  .projects-section {
    overflow: auto;
  }
  .project-boxes {
    overflow-y: visible;
  }
  .app-content {
    padding: 16px 12px 24px 12px;
  }
  .view-btn {
    width: 24px;
    height: 24px;
  }
  .app-header {
    padding: 16px 10px;
  }
  .search-input {
    max-width: 120px;
  }
  .projects-section {
    padding: 24px 16px 0 16px;
  }
  .app-header {
    padding: 10px;
  }
  .projects-section-header p {
    font-size: 18px;
  }
  .search-input {
    font-size: 14px;
  }
}

.app-right {
  background-color: var(--projects-section);
  flex-basis: 400px;
  height: 100%;
  width:100%;
  display: flex;
  flex-direction: column;
}
.app-right-header {
  display: flex;
  justify-content: flex-end;
  padding: 40px 32px 0px 32px;
}
.app-right-content {
  overflow-y: auto;
  padding: 32px;
}

.action-button {
  border-radius: 10px;
  width: 48px;
  height: 48px;
  border: none;
  padding: 0;
  margin-left: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--main-color);
  color: var(--main-color);
  flex-shrink: 0;
  transition: 0.2s linear;
  cursor: pointer;
  display: none;
}
.action-button.active {
  background-color: var(--main-active);
}
.action-button svg {
  width: 20px;
  height: 20px;
}

.product-list {
  list-style-type: none;
  padding: 0;
}
.product-list-item {
  margin-bottom: 16px;
  background-color: var(--product-list-item-bg);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-height: 100px;
  overflow: hidden;
  -webkit-animation: listItems 0.6s 0.2s both;
  animation: listItems 0.6s 0.2s both;
  transition: 0.2s linear;
  cursor: pointer;
}
.product-list-item:hover {
  background-color: #141d2b;
}
.product-list-itemContent {
  padding: 16px;
  display: flex;
  flex: 1;
  justify-content: space-between;
}

@-webkit-keyframes listItems {
  0% {
    opacity: 0;
    transform: scale(0.8);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes listItems {
  0% {
    opacity: 0;
    transform: scale(0.8);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}
.product-img-wrapper {
  overflow: hidden;
  flex-shrink: 0;
  flex-basis: 120px;
  border-radius: 12px;
  height: 100%;
}
.product-img-wrapper:hover .product-image {
  transform: scale(1.1);
}

.product-image {
  width: 100%;
  height: auto;
  -o-object-fit: cover;
  object-fit: cover;
  transition: 0.2s linear;
}

.product-amount {
  font-size: 14px;
  line-height: 24px;
  color: #fff;
}
.product-amount-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.product-amount-button {
  background-color: transparent;
  border: none;
  padding: 0;
  color: #fff;
  height: 16px;
  cursor: pointer;
  transition: 0.2s linear;
}
.product-amount-button:hover {
  color: var(--main-color-2);
}
.product-amount-button svg {
  width: 16px;
  height: auto;
}

.product-info-header {
  color: #fff;
  margin: 0 0 8px 0;
  font-size: 16px;
  line-height: 24px;
  font-weight: 500;
  max-width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.product-info-subheader {
  font-size: 14px;
  line-height: 16px;
  color: #fff;
}
.product-info-subheader span {
  font-size: 12px;
  opacity: 0.75;
}

.checkout-button {
  margin-top: auto;
  border: none;
  padding: 16px 8px;
  color: var(--main-color);
  background-color: var(--app-container);
  position: relative;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  transition: 0.2s linear;
  cursor: pointer;
  text-transform: uppercase;
  font-size: 14px;
  line-height: 16px;
}
.checkout-button:before {
  content: "";
  display: inline-block;
  vertical-align: sub;
  width: 16px;
  height: 16px;

  margin-right: 4px;
}

.product-details {
  border-radius: 16px;
  padding-top: 16px;
  background-color: var(--product-list-item-bg);
  margin-bottom: 24px;
  -webkit-animation: listItems 0.6s 0.2s both;
  animation: listItems 0.6s 0.2s both;
}
.product-details-line {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 0 16px;
  margin-bottom: 16px;
}
.product-details-text {
  display: block;
  color: #fff;
  font-size: 14px;
  line-height: 16px;
}
.product-details-text.amount {
  margin-left: auto;
}
.product-details-link {
  color: #fff;
  opacity: 0.6;
  font-size: 12px;
  line-height: 16px;
  margin-left: 8px;
  transition: 0.2s linear;
}
.product-details-link:hover {
  opacity: 1;
}
.product-details-summary {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.3);
}
.product-details-summary .product-details-text {
  font-size: 20px;
}

.app-right-hide {
  position: absolute;
  top: 12px;
  right: 12px;
  width: 24px;
  height: 24px;
  background-color: transparent;
  border: none;
  padding: 0;
  color: var(--main-color);
  display: none;
}

@media screen and (max-width: 1200px) {
  .app-content-field {
    flex-direction: column;
    min-height: unset;
    max-height: unset;
  }

  .product-box.medium {
    min-height: 120px;
    margin-bottom: 24px;
  }

  .product-box-wrapper.three .product-box:first-child,
  .product-box-wrapper.two .product-box:first-child {
    margin-left: 0;
  }
}
@media screen and (max-width: 1920px) {
  .app-container {
    position: relative;
  }

  .product-box.medium {
    height: 150px;
  }

  .product-box-wrapper.three {
    height: 150px;
  }

  .product-boxes {
    flex: inherit;
  }

  .app-header {
    display: flex;
    justify-content: space-between;
    padding: 24px 16px 0;
  }

  .app-left-content-header,
  .app-content-field {
    padding: 0 16px;
  }

  div.app-left {
    padding: 0;
  }

  .app-right-hide {
    display: block;
  }

  .action-button {
    display: flex;
  }

  .app-filter-wrapper {
    margin-left: 0;
  }

  .app-content-field.second .product-box-wrapper {
    padding-top: 0;
  }

  .app-right {
    display: none;
    position: absolute;
    height: 0;
    transition: 0.2s linear;
    opacity: 0;
    z-index: 2;
    right: -100%;
    height: 100%;
  }
  .app-right.isOpen {
    display: flex;
    height: 100%;
    right: 0px;
    opacity: 1;
  }

  .app-left {
    padding: 16px;
  }

  .product-box-wrapper.two {
    flex: unset;
    margin-bottom: 24px;
  }
}

@media screen and (max-width: 880px) {
  .app-container {
    position: relative;
  }

  .product-box.medium {
    height: 150px;
  }

  .product-box-wrapper.three {
    height: 150px;
  }

  .product-boxes {
    flex: inherit;
  }

  .app-header {
    display: flex;
    justify-content: space-between;
    padding: 24px 16px 0;
  }

  .app-left-content-header,
  .app-content-field {
    padding: 0 16px;
  }

  div.app-left {
    padding: 0;
  }

  .action-button {
    display: flex;
  }

  .app-filter-wrapper {
    margin-left: 0;
  }

  .app-content-field.second .product-box-wrapper {
    padding-top: 0;
  }

  .app-left {
    padding: 16px;
  }

  .product-box-wrapper.two {
    flex: unset;
    margin-bottom: 24px;
  }
}

@media screen and (max-width: 520px) {
  .product-img-wrapper {
    flex-basis: 80px;
  }

  .product-box-wrapper.two {
    margin-bottom: 0;
  }

  .app-right-content {
    padding: 32px 16px;
  }

  .product-box-wrapper {
    flex-direction: column;
  }

  .product-box-wrapper.three .product-box {
    width: 100%;
    margin-left: 0;
  }

  .product-box.medium {
    height: 120px;
    flex: unset;
  }

  .product-box-wrapper.three {
    height: unset;
  }

  .product-boxes {
    flex: unset;
  }

  .product-box-wrapper.two .product-box {
    width: 100%;
    margin-left: 0;
  }

  .product-box-wrapper.two {
    flex: unset;
    max-height: unset;
  }

  .product-box-wrapper.three {
    flex: unset;
    height: unset;
  }

  .app-content-field.second {
    height: unset;
    flex-shrink: 0;
    padding-top: 24px;
  }

  .app-content-field {
    flex-shrink: 0;
  }

  .app-left-content {
    display: flex;
    flex-direction: column;
  }

  .app-content-field {
    height: auto;
  }

  .content-title {
    font-size: 24px;
  }

  .app-filter-wrapper {
    max-width: 220px;
  }

  .app-left {
    padding: 8px;
  }

  .filter-dropdown-button {
    max-width: 90px;
  }

  .filter-search-input {
    max-width: calc(100% - 90px);
  }


  .product-box-wrapper.two .product-box:first-child {
    margin-top: 0;
  }

}
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
import axios from "axios";

export default {
  name: "BeerGallery",
  components: {
    BeerCard,
    SearchBar,
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
  watch: {},
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
        .get("https://api.punkapi.com/v2/beers?page=1&per_page=30")
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
        .get("https://api.punkapi.com/v2/beers?page=" + page + "&per_page=30")
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
    darkmodeToggle() {
      var modeSwitch = document.querySelector(".mode-switch");

      document.documentElement.classList.toggle("dark");
      modeSwitch.classList.toggle("active");
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
