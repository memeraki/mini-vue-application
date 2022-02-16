<template>
  <div class="main">
    <div v-if="loading" class="loading">Loading...</div>
    <div v-else class="recipes">
      <template v-for="recipe of recipes" :key="recipe.idMeal">
        <Recipe 
          :meal="recipe"
          @click="showDetails(recipe.idMeal)"
          :class="selected == recipe.idMeal || selected == 0 ? '' : 'not-selected'"
        />
        <Details 
          :meal="recipe"
          v-if="selected == recipe.idMeal" 
        />
      </template>
    </div>
  </div>
</template>

<script>
import Recipe from './Recipe.vue';
import Details from './Details.vue';

export default {
  props: ['recipes', 'loading', 'error'],
  components: {
    Recipe,
    Details
  },
  data() {
    return {
      selected: '0'
    }
  },
  methods: {
    showDetails(idMeal) {
      if(this.selected == idMeal) {
        this.selected = 0
      } else {
        this.selected = idMeal;
      }
    }
  }
}
</script>

<style>
  .main {
    grid-area: main;
    height: 100%;
    overflow-y: scroll;
  }
  .recipes {
    padding: 3vw;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: auto; 
    gap: 3vw;
    grid-auto-flow: dense;
  }
  .not-selected {
    opacity: 0.5;
  }
</style>
