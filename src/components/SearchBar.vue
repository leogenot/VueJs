<template>
  <div class="options-wrapper">
    <div class="search-wrapper">
      <input
        class="search-input"
        type="text" :value="search" @input="onSearchChanged"
        placeholder="Search for a beer"
      />
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="20"
        height="20"
        fill="none"
        stroke="currentColor"
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        class="feather feather-search"
        viewBox="0 0 24 24"
      >
        <defs></defs>
        <circle cx="11" cy="11" r="8"></circle>
        <path d="M21 21l-4.35-4.35"></path>
      </svg>
    </div>
    <div class="select-dropdown">
      <select :value="beersSortType" @input="onBeersSortTypeChanged" id="beer-sort">
        <option value="AZName">Sort from A to Z</option>
        <option value="ZAName">Sort from Z to A</option>
        <option value="PlusABV">Sort from least alcohol</option>
        <option value="MinusABV">Sort from most alcohol</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: {
		search: String,
		beersSortType: String
  },
  watch: {
    search: function (newSearch) {
      localStorage.setItem("search", newSearch);
    },
    beersSortType: function (newbeersSortType) {
      localStorage.setItem("beersSortType", newbeersSortType);
    },
  },
  computed: {
    inputVal: {
      get() {
        return this.value;
      },
      set(val) {
        this.$emit("input", val);
      },
    },
  },

  methods: {
    onSearchChanged(event) {
			this.$emit('update:search', event.target.value) 
		},
		onBeersSortTypeChanged(event) {
			this.$emit('update:beersSortType', event.target.value) 
		},
  },
};
</script>

<style>
@import url("../assets/SearchBar.css");
</style>
