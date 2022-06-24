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
          >You need to Login to vue this</router-link
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

.FavoriteRecipes {
  // margin: 10px 0 10px;
  margin: auto;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

#logo {
  height: 245px;
  margin: auto;
  display: block;
}
</style>
