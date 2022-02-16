<template>
  <div class="main">
    <div v-if="loading" class="loading">Loading...</div>
    <div v-else class="recipes">
      <Recipe 
        v-for="recipe of recipes" 
        :key="recipe.idMeal" 
        :meal="recipe"  
      />
    </div>
  </div>
</template>

<script>
import Recipe from './Recipe.vue';

export default {
  props: ['recipes', 'loading', 'error'],
  components: {
    Recipe,
  }
}
</script>

<style>
  .main {
    grid-area: main;
    height: 100%;
    overflow-y: scroll;
    background-color: chocolate;
  }
  .recipes {
    padding: 3vw;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: auto; 
    gap: 3vw;
  }
  .red {
    z-index: 999999999;
    background-color: red;
    padding: 3px;
  }
</style>
