<template>
  <div class="recipe">
    <figure>
      <img :src="meal.strMealThumb" :alt="meal.strMeal">
      <figcaption>{{ meal.strMeal }}</figcaption>
    </figure>
  </div>
</template>

<script>
export default {
  props: ['meal'] 
  
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
