<template>
  <div class="favourites" @mouseleave="isHidden = !isHidden">
    <div class="show" @mouseover="update">
      Favourites <span>&#10084;</span>
    </div>
    <div class="list" v-if="!isHidden">
      <div class="list-item" 
        v-for="(favourite, index) in favourites" 
        :key="index"
        @click="selected(favourite.name)"
        >
          <img :src="favourite.img" :alt="favourite.name" >
          <div> {{ favourite.name }} </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['recipes'],
  data() {
    return {
      favourites: [],
      isHidden: true
    }
  },
  methods: {
    update() {
      this.isHidden = !this.isHidden;
      this.favourites = [];
      const localStorageData = JSON.parse(localStorage.getItem("Favourites")) || {
        recipes: []
      };
    this.recipes.forEach(recipe => {
      localStorageData.recipes.forEach(el => {
        if(recipe.idMeal == el) {
          let rec = { img: recipe.strMealThumb, name: recipe.strMeal }
          this.favourites.push(rec);
        }
      });
    });
    },
    selected(event) {
      this.$emit('clicked', event);
    }
  }
}
</script>

<style>
.favourites {
  position: relative;
  background-color: white;
  border-radius: 15px;
  flex-basis: 100%;
  padding: 5px;
  height: 25px;
  z-index: 2;
  text-transform: uppercase;
}
.show {
  border-radius: 15px;
  text-transform: uppercase;
}
span {
  position: absolute;
  right: 10px;
  color: red;
}
.list {
  position: absolute;
  top: 50%;
  left: 0;
  /* height: 100px; */
  width: 100%;
  background-color: white;
  z-index: -1;
  border-radius: 15px;
  padding: 20px;
}
.list img {
  height: 100%;
}
.list-item {
  cursor: pointer;
  height: 30px;
  font-size: 12px;
  text-align: left;
  display: flex;
  flex-flow: row nowrap;
  justify-content: flex-start;
  margin-top: 5px;
}
.list-item div {
  margin-left: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>