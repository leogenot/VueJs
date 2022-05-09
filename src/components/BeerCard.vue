<template>
  <div class="wrapper">
    <div class="container">
      <div class="beer-img top">
        <img :src="img" height="300" class="beer-image" />
      </div>
      <div class="bottom">
        <div class="left">
          <div class="details">
            <h1>{{ name }}</h1>
            <p>{{ abv }}%</p>
          </div>
          <div class="favorite">
            <i @click="addToFavorite" class="material-icons no_fav"
              >favorite_border</i
            >
          </div>
        </div>
        <div class="right">
          <div class="done"><i class="material-icons">done</i></div>
          <div class="details">
            <h1>{{ name }}</h1>
            <p>Added to your favorites</p>
          </div>
          <div class="remove"><i class="material-icons">clear</i></div>
        </div>
      </div>
    </div>
    <div class="inside">
      <div class="icon"><i class="material-icons">info_outline</i></div>
      <div class="contents">
        <table>
          <tr>
            <th>{{ tagline }}</th>
          </tr>
          <br />
          <tr>
            <td>{{ desc }}</td>
          </tr>
          <br />
          <tr>
            <th>Food pairings</th>
          </tr>
          <tr>
            <td>
              <ul>
                <li v-for="item in food" :key="item">
                  {{ item }}
                </li>
              </ul>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>
<script>
//ONLY KEEP CLEAR FOR DEV PURPOSES DO NOT FORGET TO REMOVE IT AFTER
localStorage.clear();
// get favorites from local storage or empty array
var favorites = JSON.parse(localStorage.getItem("favorites")) || [];
// add class 'fav' to each favorite
favorites.forEach(function (favorite) {
  document.getElementById(favorite).className = "fav";
});

export default {
  name: "BeerCard",
  props: {
    img: String,
    name: { type: String, required: true },
    abv: Number,
    tagline: String,
    desc: String,
    tips: String,
    food: Array,
  },
  computed: {},
  methods: {
    addToFavorite(e) {
      var id = this.name,
        index = favorites.indexOf(id);
      if (!id) return;
      // item is not favorite
      if (index == -1) {
        favorites.push(id);
        e.target.innerHTML = "favorite"
      } else {
        favorites.splice(index, 1);
        e.target.innerHTML = "favorite_border"
      }
      localStorage.setItem("favorites", JSON.stringify(favorites));
      console.log(favorites);
    },
  },
};
</script>

<style>

@import url("../assets/BeerCard.css");

</style>
