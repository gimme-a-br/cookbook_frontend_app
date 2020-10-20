<template>
  <div class="recipes-edit">
    <form v-on:submit.prevent="updateRecipe()">
      <h1>Recipes edit</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>title:</label> 
        <input type="text" class="form-control" v-model="recipe.title">
      </div>
      <div class="form-group">
        <label>chef:</label> 
        <input type="text" class="form-control" v-model="recipe.chef">
      </div>
      <div class="form-group">
        <label>prepTime:</label> 
        <input type="text" class="form-control" v-model="recipe.prep_time">
      </div>
      <div class="form-group">
        <label>ingredients:</label> 
        <input type="text" class="form-control" v-model="recipe.ingredients">
      </div>
      <div class="form-group">
        <label>directions:</label> 
        <input type="text" class="form-control" v-model="recipe.directions">
      </div>
      <div class="form-group">
        <label>imageUrl:</label> 
        <input type="text" class="form-control" v-model="recipe.image_url">
      </div>
      <input type="submit" class="btn btn-primary" value="Submit">
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      recipe: {},
      errors: [],
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
    updateRecipe: function () {
      var params = {
        title: this.recipe.title,
        chef: this.recipe.chef,
        prep_time: this.recipe.prep_time,
        ingredients: this.recipe.ingredients,
        directions: this.recipe.directions,
        image_url: this.recipe.image_url,
      };
      axios
        .patch("/api/recipes/" + this.$route.params.id, params)
        .then((response) => {
          this.$router.push("/recipes");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
