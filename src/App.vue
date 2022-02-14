<template>
  <div class="mini-app">
    <Header />
    <SideBar 
      :categories="categories"
      :areas="areas"
      :tags="tags"
    />
    <RecipesList 
      :recipes="filteredRecipes"
      :loading="loading"
      :error="error"
    />
    <div class="try">
      <input type="text" v-model="name" placeholder="Filter By Name"/>
      <ul>
        <li v-for="recipe in filteredRecipes" :key="recipe.idMeal">{{recipe.strMeal}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import SideBar from './components/SideBar.vue';
import RecipesList from './components/RecipesList.vue';

export default {
  components: {
    Header, 
    SideBar,
    RecipesList
  },
  data() {
    return {
      loading: false,
      recipes: [],
      error: null,
      categories: [],
      areas: [],
      tags: [],
      name: ""
    }
  },
  async created() {
    this.loading = true;
    const response = await fetch("https://www.themealdb.com/api/json/v1/1/search.php?s=Soup");
    if (response.status >= 200 && response.status <= 299) {
      const jsonResponse = await response.json();
      this.recipes = jsonResponse.meals;
    } else {
      this.error = "Problem with fetching data. " + response.status + " " + response.statusText; 
    }
    this.loading = false;
    this.updateSideBar();
  },
  computed: {
    filteredRecipes() {
      return this.recipes.filter(recipe => {
        return recipe.strMeal.toLowerCase().indexOf(this.name.toLowerCase()) > -1
      });
    }
  },
  methods: {
    updateSideBar() {
      this.recipes.forEach(recipe => {
        if(!this.categories.includes(recipe.strCategory)) this.categories.push(recipe.strCategory);
        if(!this.areas.includes(recipe.strArea)) this.areas.push(recipe.strArea);
        if(recipe.strTags) {
          const tagsArray = recipe.strTags.split(',');
          tagsArray.forEach(tag => {
            if(!this.tags.includes(tag)) this.tags.push(tag);
          })
        }
      });
    } 
  },
}
</script>

<style>

html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}

* {
  padding: 0;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.mini-app {
  width: 100%;
  height: 100vh;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 100px auto;
  grid-template-areas: 
    "header header header header"
    "sidebar main main main"
    "sidebar main main main"
    "sidebar main main main";
}
</style>
