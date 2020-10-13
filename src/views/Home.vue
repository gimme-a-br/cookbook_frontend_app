<template>
  <div class="home">
    <h1>All recipes</h1>
    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <p>Chef: {{ recipe.chef }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
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
