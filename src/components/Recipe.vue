<template>
  <div class="recipe">
    <figure>
      <img :src="meal.strMealThumb" :alt="meal.strMeal" >
      <figcaption>
        {{ meal.strMeal }}
      </figcaption>
    </figure>
    <div>
      <input type="checkbox" :id="meal.idMeal" :value="meal.idMeal" v-model="favourites">
      <label :for="meal.idMeal" @click="update">heart</label>
    </div>
  </div>
</template>

<script>
export default {
  props: ['meal'],
  data() {
    return {
      favourites: [],
      favo: localStorage.getItem("Favourites")
    }
  },
  computed: {
    
  },
  methods: {
    update() {
      const fav = JSON.parse(localStorage.getItem("Favourites")) || {
        recipes: []
      };
      const index = fav.recipes.indexOf(this.meal.idMeal)
      if(index > -1) {
        fav.recipes.splice(index, 1);
      } else {
        fav.recipes.push(this.meal.idMeal);
      }
      localStorage.setItem("Favourites", JSON.stringify(fav));
    }
  }
}
</script>

<style>
.recipe {
  border-radius: 10px;
  border: 1px solid black;
  overflow: hidden;
}
img {
  width: 100%;
}
</style>
