<template>
  <div class="beer-card">



      <div class="beer-img">
        <img :src="img" height="350" />
      </div>
      <div class="beer-info">
        <h2>{{ name }} {{ abv }}%</h2>
        <p class="bright">{{ tagline }}</p>
        <p><span class="bright">Description:</span> {{ desc }}</p>
        <p><span class="bright">Tips:</span> {{ tips }}</p>
        <h3 class="bright">Food Pairings</h3>
        <ul>
          <li v-for="item in food" :key="item">
            {{ item }}
          </li>
        </ul>
      </div>
  </div>

</template>

<script>
export default {
  name: "BeerCard",
  props: {
    img: String,
		name: {type: String, required: true},
		abv: Number, 
    tagline: String,
    desc: String,
    tips: String,
    food: Object,
	},
  
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Old+Standard+TT:400i,700|Open+Sans:400,400i");


.container {
  position: relative;
  min-height: 100vh;
  width: 100vw;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  align-items: center;
}

.beer-card {
  position: relative;
  min-height: 363.4px;
  max-width: 790px;
  padding: 3.2rem;
  display: grid;
  grid: auto/1fr 3fr;
  grid-gap: 3.2rem;
  grid-template-areas: "image content";
  background-color: #fdfdfd;
  border-radius: 0;
}

.beer-image {
  height: 363.4px;
  width: auto;
  grid-area: image;
  display: flex;
  justify-content: center;
  align-items: center;
}
.beer-image img {
  max-height: 100%;
  max-width: 100%;
  -o-object-fit: contain;
     object-fit: contain;
}

.beer-name h3 {
  margin: 0 auto 0.5rem auto;
  transform: translate(-0.05em, 0);
  font-family: "Old Standard TT", serif;
  font-size: 2.4rem;
  line-height: 1.07em;
  letter-spacing: -0.015em;
}

.beer-tagline {
  border-bottom: 2px solid .4ac;
}
.beer-tagline p {
  margin-top: 0;
  font-family: "Old Standard TT", serif;
  font-style: italic;
}

.content {
  grid-area: content;
}

.beer-description p {
  margin-bottom: 0;
  font-size: 0.93rem;
  line-height: 1.3em;
}

.beer-food-pairing p {
  margin-bottom: 0;
  font-style: italic;
  font-size: 0.93rem;
  line-height: 1.3em;
}


@media all and (-webkit-min-device-pixel-ratio: 1) {
  .beer-name h3 {
    letter-spacing: -0.025em;
  }
}
@media all and (max-width: 790px) {
  .container {
    min-height: auto;
  }

  .beer-card {
    min-height: 80vh;
    max-width: 92vw;
    margin: 1.6rem 0;
    padding: 2.2624rem;
    padding-bottom: 3.2rem;
    grid: auto/auto;
    grid-gap: 0;
    grid-template-areas: "content";
  }

  .beer-image {
    display: none;
  }
}
</style>
