<template>
  <div
    class="FavoritePage"
    :style="{
      background:
        'url(' + require('@/../resources/background.jpg') + ')  no-repeat',
      backgroundSize: '410px',
    }"
  >
    <div class="FavoritePage">
      <div class="container">
        <FavoriteRecipePreviewList
          class="FavoriteRecipes center"
          title="Your Favorite Recipes"
        />
        <router-link v-if="!$root.store.username" to="/login" tag="button"
          >You need to Login to view this</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
import FavoriteRecipePreviewList from "../components/FavoriteRecipePreviewList";
export default {
  mounted() {
    console.log("favorite page mounted");
  },
  components: {
    FavoriteRecipePreviewList,
  },
};
</script>

<style lang="scss" scoped>
container {
  padding: 20px;
  font-family: Helvetica;
}

.container {
  min-height: 800px;
}

.FavoritePage {
  height: 1000px;
}

.FavoriteRecipes {
  // margin: 10px 0 10px;
  margin: auto;
}
</style>
