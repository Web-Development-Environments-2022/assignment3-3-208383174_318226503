<template>
  <b-container>
    <!-- <h4>
      {{ title }}
      <slot></slot>
    </h4> -->
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
  name: "RecipePreviewListLastSeen",
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
        const response = await this.axios
          .create({ withCredentials: true })
          .get(DOMAIN_PATH + "/users/lastThreeViewed", {
            withCredentials: true,
          });
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
h4 {
  font-size: 27px;
  font-family: Andale Mono, monospace;
  margin-bottom: 10px;
  margin-top: 0px;
}

.col {
  padding-right: 0;
  flex-grow: 0;
}
</style>
