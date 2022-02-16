<template>
  <div class="main">
    <div v-if="loading" class="loading">Loading...</div>
    <div v-else class="recipes">
      <template v-for="recipe of recipes" :key="recipe.idMeal">
        <Recipe 
          :meal="recipe"  
        />
        <div class="details">Details about: {{recipe.idMeal}}</div>
      </template>
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
    grid-auto-flow: dense;
  }
  .details {
    background-color: red;
    padding: 3px;
    grid-column: span 3; /* fro 3 columns! */
    /* grid-column: 1 / -1;  try for more ?*/
  }
</style>
