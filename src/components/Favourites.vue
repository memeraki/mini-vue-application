<template>
  <div class="favourites" @mouseleave="isHidden = !isHidden">
    <div class="show" @mouseover="update">
      <span class="fav-text">Favourites</span> <span class="fav-icon">&#10084;</span>
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
  padding: 7px;
  height: 30px;
  z-index: 2;
  text-transform: uppercase;
}
.show {
  border-radius: 15px;
  text-transform: uppercase;
}
.fav-icon {
  position: absolute;
  right: 10px;
  color: #DA3C3C;
  font-size: 20px;
  margin-top: -6px;
}
.list {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  background-color: white;
  z-index: -1;
  border-radius: 15px;
  padding: 6vw 3vw 2vw 3vw;
  box-shadow: 0px 2px 3px 0px #222222;
}
.list img {
  height: 40px;
}
.list-item {
  cursor: pointer;
  height: max-content;
  font-size: 15px;
  text-align: left;
  display: flex;
  flex-flow: row nowrap;
  justify-content: flex-start;
  align-items: center;
  margin: 2vw 0;
}
.list-item div {
  margin-left: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
}
/* mobile */
  @media (max-width: 780px) {
    .favourites {
      font-size: 15px;
    }
    .fav-text {
      display: none;
    }
    .fav-icon {
      right: 50%;
      transform: translateX(50%);
    }
    .list-item {
      flex-flow: column nowrap;
      align-items: center;
      font-size: 10px;
      text-align: center;
    }
    .list img {
      height: auto;
      width: 35%;
    }
    .list-item div {
      margin-top: 5px;
    }
  }
</style>