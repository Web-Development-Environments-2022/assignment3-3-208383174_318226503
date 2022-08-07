<template>
  <b-container>
    <b-row>
      <div class="col-sm">
        <h4>Explore this recipes</h4>
        <b-col v-for="r in randomRecipes" :key="r.id">
          <RecipePreviewHorizontal class="recipePreview" :recipe="r" />
        </b-col>
      </div>
      <div class="col-sm">
        <h4>Last Seen</h4>
        <b-col v-for="r in lastSeenRecipes" :key="r.id">
          <RecipePreviewHorizontal class="recipePreview" :recipe="r" />
        </b-col>
      </div>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreviewHorizontal from "./RecipePreviewHorizontal.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreviewHorizontal,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    list_type: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      randomRecipes: [],
      lastSeenRecipes: [],
    };
  },
  mounted() {
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
        this.lastSeenRecipes = [];
        this.lastSeenRecipes.push(...recipes);
      } catch (error) {
        console.dir("error at recipe preview list");
        console.dir(error);
      }

      try {
        const response = await this.axios.get(DOMAIN_PATH + "/recipes/random", {
          withCredentials: true,
        });
        const recipes = response.data;
        this.randomRecipes = [];
        this.randomRecipes.push(...recipes);
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
  text-align: center;
}

.col {
  padding: 0;
  flex-grow: 0;
}
</style>
