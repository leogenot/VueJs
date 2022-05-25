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
// get favorites from local storage or empty array
var favorites = JSON.parse(localStorage.getItem("favorites")) || [];
// add class 'fav' to each favorite
/* favorites.forEach(function (favorite) {
  document.getElementById(favorite).className = "fav";
}); */

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
    beersArray: Array,
    favoritesArray_prop: Array,
  },
  data() {
    return {
      favoritesArray: this.favoritesArray_prop,
    };
  },
  computed: {},
  methods: {
    getObject(object, value) {
      var result;
      return (
        (Object.keys(object).some(function (k) {
          if (object[k] === value) {
            result = object;
            return true;
          }
          if (
            object[k] &&
            typeof object[k] === "object" &&
            (result = this.getObject(object[k], value))
          ) {
            return true;
          }
        }) &&
          result) ||
        undefined
      );
    },

    addToFavorite(e) {
      //ONLY KEEP CLEAR FOR DEV PURPOSES DO NOT FORGET TO REMOVE IT AFTER
      //localStorage.clear();


      var id = this.name,
        index = favorites.indexOf(id);
      //console.log(id)

      if (!id) return;
      // item is not favorite
      if (index == -1) {
        favorites.push(id);
        e.target.innerHTML = "favorite";
        for (let i = 0; i < this.beersArray.length; i++) {
          if (this.beersArray[i].name == id) {
            this.favoritesArray.push(this.beersArray[i])
            this.$emit(
              "update:favoritesArray",
              this.favoritesArray
            );
            //console.log(this.favoritesArray);
          }
        }
      } else {
        favorites.splice(index, 1);

        e.target.innerHTML = "favorite_border";
        for (let i = 0; i < this.beersArray.length; i++) {
          if (this.beersArray[i].name == id) {
            console.log(id)
            this.favoritesArray.splice(this.beersArray[i], 1)
            this.$emit(
              "update:favoritesArray",
              this.favoritesArray
            );
            //console.log(this.favoritesArray);
          }
        }
      }
      localStorage.setItem("favorites", JSON.stringify(this.favoritesArray));
      console.log(this.favoritesArray);
    },
  },
};
</script>

<style>
@import url("../assets/BeerCard.css");
</style>
