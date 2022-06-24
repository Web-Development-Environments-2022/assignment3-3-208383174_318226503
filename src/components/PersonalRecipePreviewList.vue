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
        <b-alert v-if="!this.hasPersonal" show variant="danger"><a class="alert-link">You don't have personal recipes yet</a></b-alert>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "PersonalRecipePreviewList",
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
      hasPersonal: true,
    };
  },
  mounted() {
    console.log("PersonalRecipePreviewList mounted");
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      const DOMAIN_PATH = "http://localhost:3000";
      try {
        const response = await this.axios.get(
          DOMAIN_PATH + "/users/myRecipes",
          { withCredentials: true }
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        if (response.status==200){
            const recipes = response.data;
            this.recipes = [];
            this.recipes.push(...recipes);
        }
        else if(response.status==204){
            // no fav recipes
            this.hasPersonal = false;
        }

      } catch (error) {
        console.dir("error at PersonalRecipePreviewList");
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
}

.col {
  padding-right: 0;
}
</style>
