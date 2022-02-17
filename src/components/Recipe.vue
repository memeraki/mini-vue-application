<template>
  <div class="recipe">
    <div class="img">
      <img :src="meal.strMealThumb" :alt="meal.strMeal" >
      <p> {{ meal.strMeal }} </p>
    </div>
    <div class="heart">
      <input type="checkbox" :id="meal.idMeal" :value="meal.idMeal" v-model="checked">
      <label :for="meal.idMeal" @click="update">&#10084;</label>
    </div>
  </div>
</template>

<script>
export default {
  props: ['meal'],
  data() {
    return {
      checked: false,
      favourites: [],
    }
  },
  created() {
    const fav = JSON.parse(localStorage.getItem("Favourites")) || {
        recipes: []
      };
    this.favourites = fav.recipes;
    if(this.favourites.indexOf(this.meal.idMeal) > -1) {
      this.checked = true;
    }
  },
  methods: {
    update() {
      const localStorageData = JSON.parse(localStorage.getItem("Favourites")) || {
        recipes: []
      };
      const index = localStorageData.recipes.indexOf(this.meal.idMeal)
      if(index > -1) {
        localStorageData.recipes.splice(index, 1);
      } else {
        localStorageData.recipes.push(this.meal.idMeal);
      }
      localStorage.setItem("Favourites", JSON.stringify(localStorageData));
    }
  }
}
</script>

<style>
.recipe {
  position: relative;
  border-radius: 15px;
  border: 1px solid #222222;
  overflow: hidden;
  height: 200px;
}
.img img{
  width: 100%;
}
.img p {
  background-color: white;
  text-align: center;
  width: 100%;
  height: 50px;
  font-size: 10;
  line-height: 14px;
  position: absolute;
  bottom: 0;
  text-transform: uppercase;
  display: flex;
  justify-content: center;
  align-items: center;
}
.heart {
  cursor: pointer;
  font-size: 25px;
  color: white;
  position: absolute;
  bottom: 50px;
  right: 10px;
}
.heart input[type="checkbox"] {
    opacity: 0;
    width: 0;
    height: 0;
}
.heart input[type="checkbox"]:checked + label {
  color: #DA3C3C; 
}
/* mobile */
@media (max-width: 780px) {
  .recipe {
    height: 150px;
  }
}
</style>
