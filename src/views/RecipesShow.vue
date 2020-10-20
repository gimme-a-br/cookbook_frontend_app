<template>
  <div class="recipes-show">
    <h1>Recipe info</h1>

    <div class="card mb-3" style="max-width: 540px;">
      <div class="row no-gutters">
        <div class="col-md-4">
          <img v-bind:src="recipe.image_url" class="card-img" alt="">
        </div>
        <div class="col-md-8">
          <div class="card-body">
            <h5 class="card-title">{{ recipe.title }}</h5>
            <p class="card-text"><small class="text-muted">Prep time: {{ recipe.prep_time}} minutes</small></p>
            <p class="card-text">Ingredients: {{ recipe.ingredients}}</p>
            <p class="card-text">Directions: {{ recipe.directions}}</p>
            <a class="btn btn-primary btn-block" v-bind:href="`/recipes/${recipe.id}/edit`">Edit recipe</a>

            <button class="btn btn-primary btn-block" v-on:click="destroyRecipe(recipe)">Destroy recipe</button>
          </div>
        </div>
      </div>
    </div>
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
    destroyRecipe: function (recipe) {
      axios.delete("/api/recipes/" + recipe.id).then((response) => {
        console.log("Success", response.data);
        this.$router.push("/recipes");
      });
    },
  },
};
</script>
