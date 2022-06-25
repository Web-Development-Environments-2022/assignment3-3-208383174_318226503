<template>
  <b-container>
    <h4>
      {{ title }}
      <slot></slot>
    </h4>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
    <b-row>
      <b-alert v-if="!this.hasFavorite" show variant="danger"
        ><a class="alert-link">You don't have favorite recipes yet</a></b-alert
      >
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "FavoriteRecipePreviewList",
  components: {
    RecipePreview,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      recipes: [],
      hasFavorite: true,
    };
  },
  mounted() {
    console.log("FavoriteRecipePreviewList mounted");
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      const DOMAIN_PATH = "http://localhost:3000";
      try {
        const response = await this.axios.get(
          DOMAIN_PATH + "/users/favorites",
          { withCredentials: true }
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        if (response.status == 200) {
          const recipes = response.data;
          this.recipes = [];
          this.recipes.push(...recipes);
        } else if (response.status == 204) {
          // no fav recipes
          this.hasFavorite = false;
        }
      } catch (error) {
        console.dir("error at FavoriteRecipePreviewList");
        console.dir(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
  max-width: 980px;
  margin: auto;
}
h4 {
  font-size: 27px;
  font-family: Andale Mono, monospace;
  margin-bottom: 10px;
  margin-top: 10px;
}

.col {
  flex-grow: 0;
  margin-bottom: 15px;
}

.col {
  padding-right: 0;
}
</style>
