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
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
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
    };
  },
  mounted() {
    console.log("recipe preview list mounted");
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      const DOMAIN_PATH = "http://localhost:3000";
      try {
        const response = await this.axios.get(
          DOMAIN_PATH + "/recipes/random"
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
      } catch (error) {
        console.dir("error at recipe preview list");
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
.col {
  padding-right: 0;
}
</style>
