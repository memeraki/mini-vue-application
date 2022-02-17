<template>
  <div class="details">
    <iframe class="movie" :src="convertYoutubeLink()" :title="meal.strMeal" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    <div class="text">
      <div class="instructions">
        <h2>Instruction</h2>
        <p>{{ meal.strInstructions }}</p>
      </div>
      <div class="ingredients">
        <h2>Ingredients + Measure</h2>
        <p v-for="(ingredient, index) in ingredients" :key="index">
          {{ ingredient }} - {{ measure[index] }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['meal'],
  data() {
    return {
      ingredients: [],
      measure: []
    }
  },
  created() {
    this.propsToArray();
  },
  methods: {
    convertYoutubeLink() {
      let str = [];
      str = this.meal.strYoutube.split('=');
      const link = "https://www.youtube.com/embed/" + str[1];
      return link;
    },
    propsToArray() {
      for(let i = 1; i <= 20; i++){
        if(this.meal["strIngredient"+i])
          this.ingredients.push(this.meal["strIngredient"+i]);
        if(this.meal["strMeasure"+i])
          this.measure.push(this.meal["strMeasure"+i]);
      }
    },
  }
}
</script>

<style>
  .details {
    text-align: left;
    font-size: 14px;
    grid-column: 1 / -1;  
    display: flex;
    height: auto;
    border: 1px solid #222222;
    border-radius: 15px;
    overflow: hidden;
    height: 250px;
  }
  .movie {
    height: 100%; /* ??? */
    width: auto;
    margin: 0;
    padding: 0;
  }
  .text {
    margin: 1vw;
    overflow-y: scroll;
    padding: 0 1vw 0 0;
  }
  .instructions h2 {
    margin-bottom: 15px;
  }
  .ingredients h2 {
    margin: 15px 0;
  }
  .ingredients p {
    text-transform: uppercase;
  }
  .text::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    background-color: #222;
  }
  .text::-webkit-scrollbar {
    width: 6px;
    background-color: #222;
  }
  .text::-webkit-scrollbar-thumb {
    background-color: #F7B32B;
  }
  /* mobile */
  @media (max-width: 780px) {
    .movie {
      height: auto; /* ??? */
      width: 50%;
      margin: 0;
      padding: 0;
    }
    .details {
      height: 200px;
    }
  }
</style>
