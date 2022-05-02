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
@import url("https://fonts.googleapis.com/css?family=Old+Standard+TT:400i,700|Open+Sans:400,400i");

.details {
  overflow-y: scroll;
}
.beer-image {
  margin-top: 50px;
}

.wrapper {
  width: 300px;
  height: 500px;
  background: white;
  margin: auto;
  position: relative;
  overflow: hidden;
  border-radius: 10px 10px 10px 10px;
  box-shadow: 0;
  transform: scale(0.95);
  transition: box-shadow 0.5s, transform 0.5s;
}
.wrapper:hover {
  transform: scale(1);
  box-shadow: 5px 20px 30px rgba(0, 0, 0, 0.2);
}
.wrapper .container {
  width: 100%;
  height: 100%;
}
.wrapper .container .top {
  display: flex;
  justify-content: center;
  height: 80%;
  width: 100%;
  background: no-repeat center center;
  -webkit-background-size: 100%;
  -moz-background-size: 100%;
  -o-background-size: 100%;
  background-size: 100%;
}
.wrapper .container .bottom {
  width: 200%;
  height: 20%;
  transition: transform 0.5s;
}
.wrapper .container .bottom.clicked {
  transform: translateX(-50%);
}
.wrapper .container .bottom h1 {
  font-size: 1.2em;
  margin: 0;
  padding: 0;
}
.wrapper .container .bottom p {
  margin: 0;
  padding: 0;
}
.wrapper .container .bottom .left {
  height: 100%;
  width: 50%;
  background: #f4f4f4;
  position: relative;
  float: left;
}
.wrapper .container .bottom .left .details {
  padding: 20px;
  float: left;
  width: calc(70% - 40px);
}
.wrapper .container .bottom .left .favorite {
  float: right;
  width: calc(30% - 2px);
  height: 100%;
  background: #f1f1f1;
  transition: background 0.5s;
  border-left: solid thin rgba(0, 0, 0, 0.1);
}
.wrapper .container .bottom .left .favorite i {
  font-size: 30px;
  padding: 30px;
  color: #254053;
  transition: transform 0.5s;
}
.favorite {
  cursor: pointer;
}
.wrapper .container .bottom .left .favorite:hover {
  background: #a6cdde;
}
.wrapper .container .bottom .left .favorite:hover i {
  transform: translateY(5px);
  color: #00394b;
}
.wrapper .container .bottom .right {
  width: 50%;
  background: #a6cdde;
  color: white;
  float: right;
  height: 200%;
  overflow-y: scroll;
}
.wrapper .container .bottom .right .details {
  padding: 20px;
  float: right;
  width: calc(70% - 40px);
}
.wrapper .container .bottom .right .done {
  width: calc(30% - 2px);
  float: left;
  transition: transform 0.5s;
  border-right: solid thin rgba(255, 255, 255, 0.3);
  height: 50%;
}
.wrapper .container .bottom .right .done i {
  font-size: 30px;
  padding: 30px;
  color: white;
}
.wrapper .container .bottom .right .remove {
  width: calc(30% - 1px);
  clear: both;
  border-right: solid thin rgba(255, 255, 255, 0.3);
  height: 50%;
  background: #bc3b59;
  transition: transform 0.5s, background 0.5s;
}
.wrapper .container .bottom .right .remove:hover {
  background: #9b2847;
}
.wrapper .container .bottom .right .remove:hover i {
  transform: translateY(5px);
}
.wrapper .container .bottom .right .remove i {
  transition: transform 0.5s;
  font-size: 30px;
  padding: 30px;
  color: white;
}
.wrapper .container .bottom .right:hover .remove,
.wrapper .container .bottom .right:hover .done {
  transform: translateY(-100%);
}
.wrapper .inside {
  z-index: 9;
  background: #95afca;
  width: 140px;
  height: 140px;
  position: absolute;
  top: -70px;
  right: -70px;
  border-radius: 0px 0px 200px 200px;
  transition: all 0.5s, border-radius 2s, top 1s;
  overflow-y: scroll;
}
.wrapper .inside .icon {
  position: absolute;
  right: 85px;
  top: 85px;
  color: white;
  opacity: 1;
}
.wrapper .inside:hover {
  width: 100%;
  right: 0;
  top: 0;
  border-radius: 0;
  height: 80%;
}
.wrapper .inside:hover .icon {
  opacity: 0;
  right: 15px;
  top: 15px;
}
.wrapper .inside:hover .contents {
  opacity: 1;
  transform: scale(1);
  transform: translateY(0);
}
.wrapper .inside .contents {
  overflow: scroll;
  padding: 5%;
  opacity: 0;
  transform: scale(0.5);
  transform: translateY(-200%);
  transition: opacity 0.2s, transform 0.8s;
}
.wrapper .inside .contents table {
  text-align: left;
  width: 100%;
}
.wrapper .inside .contents h1,
.wrapper .inside .contents p,
.wrapper .inside .contents table {
  color: white;
}
.wrapper .inside .contents p {
  font-size: 13px;
}
</style>
