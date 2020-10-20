<template>
  <div class="recipes-show">
    <h1>Recipe info</h1>
    <a v-bind:href="`/recipes/${recipe.id}/edit`">Edit recipe</a>
    <h2>{{ recipe.title }}</h2>
    <img v-bind:src="recipe.image_url" alt="">
    <p>Ingredients: {{ recipe.ingredients}}</p>
    <p>Directions: {{ recipe.directions}}</p>
    <p>Prep time: {{ recipe.prep_time }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RecipesShow",
  data: function () {
    return {
      recipe: {},
    };
  },
  created: function () {
    this.showRecipe();
  },
  methods: {
    showRecipe: function () {
      axios.get("/api/recipes/" + this.$route.params.id).then((response) => {
        console.log("One recipe:", response.data);
        this.recipe = response.data;
      });
    },
  },
};
</script>
