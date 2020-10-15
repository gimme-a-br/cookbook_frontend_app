<template>
  <div class="home">
    <h1>New recipe</h1>
    <div>
      Title: <input type="text" v-model="newRecipeTitle">
      Chef: <input type="text" v-model="newRecipeChef">
      Prep time: <input type="text" v-model="newRecipePrepTime">
      Ingredients: <input type="text" v-model="newRecipeIngredients">
      Directions: <input type="text" v-model="newRecipeDirections">
      Image url: <input type="text" v-model="newRecipeImageUrl">
      <button v-on:click="createRecipe()">Create</button>
    </div>
    <h1>All recipes</h1>
    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
      <p>Chef: {{ recipe.chef }}</p>
      <button v-on:click="showRecipe(recipe)">More info</button>
    </div>

    <dialog id="recipe-details">
      <form method="dialog">
        <h1>Recipe info</h1>
        <p>Title: <input type="text" v-model="currentRecipe.title"></p>
        <p>Chef: <input type="text" v-model="currentRecipe.chef"></p>
        <p>Ingredients: <input type="text" v-model="currentRecipe.ingredients"></p>
        <p>Directions: <input type="text" v-model="currentRecipe.directions"></p>
        <p>Prep time: <input type="text" v-model="currentRecipe.prep_time"></p>
        <button v-on:click="updateRecipe(currentRecipe)">Update</button>
        <button v-on:click="destroyRecipe(currentRecipe)">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>

  </div>
</template>

<style>
img {
  width: 200px;
}
</style>

<script>
import axios from "axios";

export default {
  name: "Home",
  data: function () {
    return {
      recipes: [],
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipePrepTime: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipeImageUrl: "",
      currentRecipe: {},
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
    createRecipe: function () {
      console.log("Create the recipe...");
      var params = {
        title: this.newRecipeTitle,
        chef: this.newRecipeChef,
        ingredients: this.newRecipeIngredients,
        directions: this.newRecipeDirections,
        image_url: this.newRecipeImageUrl,
        prep_time: this.newRecipePrepTime,
      };
      axios
        .post("/api/recipes", params)
        .then((response) => {
          console.log("Success", response.data);
          this.recipes.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showRecipe: function (recipe) {
      console.log("Gonna show more info...", recipe);
      this.currentRecipe = recipe;
      document.querySelector("#recipe-details").showModal();
    },
    updateRecipe: function (recipe) {
      console.log("Update this recipe", recipe);
      var params = {
        title: recipe.title,
        chef: recipe.chef,
        ingredients: recipe.ingredients,
        directions: recipe.directions,
        image_url: recipe.image_url,
        prep_time: recipe.prep_time,
      };
      axios
        .patch("/api/recipes/" + recipe.id, params)
        .then((response) => {
          console.log("Success", response.data);
        })
        .catch((error) => console.log(error.response));
    },
    destroyRecipe: function (recipe) {
      axios.delete("/api/recipes/" + recipe.id).then((response) => {
        console.log("Success", response.data);
        // Remove this recipe from this.recipes
        var index = this.recipes.indexOf(recipe);
        this.recipes.splice(index, 1);
      });
    },
  },
};
</script>
