<template>
  <div class="mini-app">
    <header class="header">
      <nav>
        <button 
          v-on:click="showSidebar = !showSidebar" >
          <img src="./assets/bars-solid.svg" alt="menu icon" />
        </button>
      </nav>
      <div class="searchBar">
        <input type="text" v-model="search" placeholder="Search" />
      </div>
      <Favourites 
        :recipes="recipes"
        @clicked="searchName"
      />
    </header>
    <div class="sidebar" v-if="!showSidebar">
      <div>
        <h2>Category</h2>
        <ul>
          <li v-for="(categoryName, index) in filtersList.categories" :key="index">
            <input type="checkbox" :id="categoryName" :value="categoryName" v-model="selectedAttributes">
            <label :for="categoryName">{{ categoryName }}</label>
          </li>
        </ul>
      </div>
      <div>
        <h2>Area</h2>
        <ul>
          <li v-for="(areaName, index) in filtersList.areas" :key="index">
            <input type="checkbox" :id="areaName" :value="areaName" v-model="selectedAttributes">
            <label :for="areaName">{{ areaName }}</label>
          </li>
        </ul>
      </div>
      <div>
        <h2>Tags</h2>
        <ul>
          <li v-for="(tagName, index) in filtersList.tags" :key="index">
            <input type="checkbox" :id="tagName" :value="tagName" v-model="selectedAttributes">
            <label :for="tagName">{{ tagName }}</label>
          </li>
        </ul>
      </div>
    </div>
    <RecipesList 
      :recipes="filteredRecipes"
      :loading="loading"
      :error="error"
    />
  </div>
</template>

<script>
import RecipesList from './components/RecipesList.vue';
import Favourites from './components/Favourites.vue';

export default {
  components: {
    RecipesList,
    Favourites
  },
  data() {
    return {
      loading: false,
      recipes: [],
      error: null,
      search: "",
      filters: { 
        categories: [],
        areas: [],
        tags: [],  
      },
      selectedAttributes: [],
      showSidebar: false
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
  },
  computed: {
    filteredRecipes() {
      // Recipets list update
      let temp = this.recipes.filter(recipe => {
        // Recipets list update by name
        return recipe.strMeal.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
      });

      return !this.selectedAttributes.length ? temp : temp.filter(recipe => {
        // Recipets list update by attribytes
        let tagsArray = [];
        if(recipe.strTags) {
          tagsArray = recipe.strTags.replace(/\s/g, '').split(',');
        }
        return this.selectedAttributes.every( att => Object.values(recipe).indexOf(att) > -1 || tagsArray.includes(att) );
      });
    },
    filtersList() {
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.filters = {
        categories: [],
        areas: [],
        tags: [],  
      }
      // Attributes update
      this.filteredRecipes.forEach(recipe => {
        if(!this.filters.categories.includes(recipe.strCategory)) this.filters.categories.push(recipe.strCategory);
        if(!this.filters.areas.includes(recipe.strArea)) this.filters.areas.push(recipe.strArea);
        if(recipe.strTags) {
          const tagsArray = recipe.strTags.replace(/\s/g, '').split(',');
          tagsArray.forEach(tag => {
            if(!this.filters.tags.includes(tag)) this.filters.tags.push(tag);
          });
        }
      });
      return this.filters;
    }
  },
  methods: {
    searchName(name) {
      this.search = name;
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
  color: #222222;
  font-size: 15px;
}
.mini-app {
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: calc(50px + (3vw)) auto;
  grid-template-areas: 
    "header header header header"
    "sidebar main main main"
    "sidebar main main main"
    "sidebar main main main";
}
.header {
    grid-area: header;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    width: 100%;
    background-color: #F7B32B;
    padding: 3vw;
  }
  .sidebar {
    grid-area: sidebar;
    padding: 3vw;
    background-color: #747C92;
    display: flex;
    flex-flow: column nowrap;
    justify-content: flex-start;
    height: 100%;
  }
  h2 {
    font-size: 1.5em;
    margin-bottom: 1vw;
    text-align: left;
  }
  h3 {
    font-size: 1.2em;
  }
  ul {
    list-style: none;
    border: 1px solid #222222;
    border-radius: 5px;
    display: flex;
    flex-flow: row wrap;
    padding: 1vw;
    margin-bottom: 3vw;
  }
  li {
    margin: 7.5px 2.5px;
  }
  .sidebar input[type="checkbox"] {
    opacity: 0;
    width: 0;
    height: 0;
  }
  .sidebar label {
    border-radius: 5px;
    padding: 5px;
    padding-left: 12px;
    position: relative;
    background-color: cornsilk;
    cursor: pointer;
  }
  .sidebar label:hover {
    background-color: #F7B32B;
  }
  .sidebar label::before {
    content: "x";
    font-variant: small-caps;
    font-size: 10px;
    position: absolute;
    top: 7px;
    left: 3px;
    height: 100%;
  }
  .sidebar input[type="checkbox"]:checked + label {
    background-color: #F7B32B;
  }
  nav {
    flex-basis: 100%;
    display: flex;

  }
  nav img {
    height: 30px;
    cursor: pointer;
  }
  .searchBar {
    flex-basis: 200%;
    margin: 3vw;
  }
  .searchBar input {
    width: 100%;
    border-radius: 15px;
    background: url("./assets/magnifying-glass-solid.svg") no-repeat scroll 10px;
    background-size: 20px;
    padding-left:35px;
    height: 30px;
    border: 1px solid #222222;
    text-align: left;
  }
  .searchBar input::placeholder {
    text-transform: uppercase;
    color: #222222;
    opacity: 0.5;
  }
  /* mobile */
  @media (max-width: 780px) {
    .mini-app {
      grid-template-areas: 
        "header header header header"
        "sidebar sidebar sidebar sidebar"
        "main main main main"
        "main main main main"
        "main main main main";
    }
    .searchBar input::placeholder {
      font-size: 12px;
    }
  }
</style>
