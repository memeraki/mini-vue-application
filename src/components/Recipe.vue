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
      favo: localStorage.getItem("fav")
    }
  },
  computed: {
    
  },
  methods: {
    update() {
      // let fav = localStorage.getItem("fav") || [];
      // jesli nie ma w fav
      const fav = [localStorage.getItem("fav"), this.meal.idMeal];
      // jesli jest to trzeba fav przefiltowac i usunac
      localStorage.setItem("fav", fav);
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
