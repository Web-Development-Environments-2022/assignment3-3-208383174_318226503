<template>
  <div>
    <b-icon
      @click="markFavorite(recipe)"
      v-if="recipe.isFavorite === false"
      class="heart_icon"
      icon="heart"
      font-scale="2"
    ></b-icon>

    <b-icon
      @click="unmarkFavorite(recipe)"
      v-if="recipe.isFavorite === true"
      class="heart_icon"
      icon="heart-fill"
      variant="danger"
      font-scale="2"
    ></b-icon>
  </div>
</template>

<script>
export default {
  name: "FavoriteButton",
  props: {
    recipe: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {};
  },
  mounted() {},

  methods: {
    async markFavorite(recipe) {
      console.log("marking as favorite");
      console.log("is personal " + recipe.isPersonal);
      const DOMAIN_PATH = "http://localhost:3000";
      try {
        await this.axios.post(
          DOMAIN_PATH + "/users/favorites",
          { recipeId: recipe.id },
          {
            params: {
              personal: recipe.isPersonal,
            },
          }
        );
        recipe.isFavorite = true;
      } catch (error) {
        if (error.response.status == 401) {
          this.$router.push("/login");
        }
        console.dir("error at marking as favorite");
        console.dir(error);
      }
    },
    async unmarkFavorite(recipe) {
      console.log("unmarking as favorite");

      const DOMAIN_PATH = "http://localhost:3000";
      try {
        await this.axios.delete(DOMAIN_PATH + "/users/favorites", {
          data: {
            recipeId: recipe.id,
            personal: recipe.isPersonal,
          },
        });
        recipe.isFavorite = false;
      } catch (error) {
        console.dir("error at unmarking as favorite");
        console.dir(error);
      }
    },
  },
};
</script>

<style scoped>
.b-icon {
  cursor: pointer;
}
</style>
