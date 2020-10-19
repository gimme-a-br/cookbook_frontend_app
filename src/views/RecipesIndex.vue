<template>
  <div class="recipes-index">
    <h1>All recipes</h1>
    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <a v-bind:href="`/recipes/${recipe.id}`">more info</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RecipesIndex",
  data: function () {
    return {
      recipes: [],
    };
  },
  created: function () {
    this.indexRecipes();
  },
  methods: {
    indexRecipes: function () {
      axios.get("/api/recipes").then((response) => {
        console.log("All recipes:", response.data);
        this.recipes = response.data;
      });
    },
  },
};
</script>
