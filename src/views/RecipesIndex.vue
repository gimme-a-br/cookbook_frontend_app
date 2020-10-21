<template>
  <div class="recipes-index">
    <h1>All recipes</h1>
    <div>
      Search recipes: <input type="text" v-model="searchFilter" list="recipe-titles">
      <datalist id="recipe-titles">
        <option v-for="recipe in recipes">{{ recipe.title }}</option>
      </datalist>
    </div>
    <div class="row row-cols-1 row-cols-md-4">
      <div v-for="recipe in filterBy(recipes, searchFilter, 'title', 'ingredients')" class="col mb-4">
        <div class="card">
          <img v-bind:src="recipe.image_url" class="card-img-top" alt="">
          <div class="card-body">
            <h5 class="card-title">{{ recipe.title }}</h5>
            <p class="card-text">
              {{ recipe.ingredients }}
            </p>
            <p>
              <a v-bind:href="`/recipes/${recipe.id}`">more info</a>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  name: "RecipesIndex",
  data: function () {
    return {
      searchFilter: "",
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
