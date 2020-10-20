<template>
  <div class="signup">
    <img v-if="status" v-bind:src="`https://http.cat/${status}`" alt="">
    <form v-on:submit.prevent="createRecipe()">
      <h1>Recipes new</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>title:</label> 
        <input type="text" class="form-control" v-model="title">
      </div>
      <div class="form-group">
        <label>chef:</label> 
        <input type="text" class="form-control" v-model="chef">
      </div>
      <div class="form-group">
        <label>prepTime:</label> 
        <input type="text" class="form-control" v-model="prepTime">
      </div>
      <div class="form-group">
        <label>ingredients:</label> 
        <input type="text" class="form-control" v-model="ingredients">
      </div>
      <div class="form-group">
        <label>directions:</label> 
        <input type="text" class="form-control" v-model="directions">
      </div>
      <div class="form-group">
        <label>imageUrl:</label> 
        <input type="text" class="form-control" v-model="imageUrl">
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
      title: "",
      chef: "",
      prepTime: "",
      ingredients: "",
      directions: "",
      imageUrl: "",
      errors: [],
      status: "",
    };
  },
  methods: {
    createRecipe: function () {
      var params = {
        title: this.title,
        chef: this.chef,
        prep_time: this.prepTime,
        ingredients: this.ingredients,
        directions: this.directions,
        image_url: this.imageUrl,
      };
      axios
        .post("/api/recipes", params)
        .then((response) => {
          this.$router.push("/recipes");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          this.status = error.response.status;
        });
    },
  },
};
</script>
