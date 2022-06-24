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
    <button class="random-button" @click="changeRandom">
      Get other recipes
    </button>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewListRandom",
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
      componentKey: 0,
    };
  },
  mounted() {
    console.log("recipe preview list mounted");
    this.updateRecipes();
  },
  methods: {
    async changeRandom() {
      this.componentKey += 1;
    },

    async updateRecipes() {
      const DOMAIN_PATH = "http://localhost:3000";
      try {
        const response = await this.axios.get(DOMAIN_PATH + "/recipes/random", {
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
.random-button {
  // margin: auto;
  // display: flow-root;
  margin-top: 24px;
  padding: 5px 13px 5px 13px;
  font-size: 19px;
  border: 0.2px solid rgb(255, 255, 255);
  // border: none;
  border-radius: 5px;
  background-color: #dca65e;
  color: white;
  font-family: Noto Sans, sans-serif;
}

.random-button:hover,
.random-button:active {
  background-color: #e79b38;
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
