<template>
  <div>
    <div class="card" style="width: 20rem; height: 25rem;">
      <router-link
        :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
        id="recipe-preview"
      >
        <div class="recipe-body">
          <img v-if="image_load" :src="recipe.image" class="card-img-top" />
        </div>
        <h5 class="card-title">
          {{ recipe.title }}
        </h5>
      </router-link>
      <div class="card-body">
        <ul class="list-group list-group-flush">
          <p id="recipe_info">
            {{ recipe.readyInMinutes }} minutes | {{ recipe.popularity }} likes
          </p>
          <p id="viewed_recipe" v-if="recipe.isViewed === true">
            you already viewed this recipe
          </p>
          <div class="food_icons">
            <FavoriteButton class="favoriteButton" :recipe="recipe" />
            <img
              id="vegetarian"
              v-if="recipe.vegetarian === true"
              src="../../resources/vegeterian.png"
              height="30px"
              width="30px"
              title="vegetarian"
            />
            <img
              id="vegan"
              v-if="recipe.vegan === true"
              src="../../resources/vegan.png"
              height="30px"
              width="30px"
              title="vegan"
            />
            <img
              id="glutenFree"
              v-if="recipe.glutenFree === true"
              src="../../resources/gluten-free.png"
              height="30px"
              width="30px"
              title="gluten free"
            />
          </div>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import FavoriteButton from "./FavoriteButton.vue";
export default {
  name: "RecipePreview",
  components: {
    FavoriteButton,
  },
  mounted() {
    console.log("recipe preview mounted");
    this.axios.get(this.recipe.image).then((i) => {
      this.image_load = true;
    });
  },
  data() {
    return {
      image_load: false,
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
#recipe-preview {
  display: inline-block;
  width: 100%;
  height: auto;
  position: relative;
  text-decoration: none;
}

.card-img-top:hover {
  opacity: 0.85;
}

.food_icons {
  position: absolute;
  top: 73px;
}

.food_icons img {
  margin-right: 10px;
  position: relative;
}

.favoriteButton {
  position: absolute;
  left: 257px;
}

.card {
  color: black;
}

.card-body {
  padding-top: 4px;
  position: absolute;
  top: 285px;
  /* padding: 1.25rem 1.2rem 1.25rem 1.2rem; */
}

.card .card-title {
  margin-top: 0px;
  margin-bottom: 10px;
  overflow: hidden;
  display: -webkit-box;
  max-width: 20rem;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  padding: 0.6rem 1.2rem 0 1.2rem;
  color: black;
}

.card .card-title:hover {
  text-decoration: none;
  color: rgb(100, 100, 100);
}

.recipe-body {
  position: relative;
  width: 100%;
}

#recipe_info {
  margin-bottom: 2px;
  margin-top: 0px;
}

#viewed_recipe {
  margin-top: 3px;
  color: rgb(165, 161, 161);
}
</style>
