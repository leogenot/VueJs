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

<style></style>
