<template>
  <div
    class="mainPage"
    :style="{
      background:
        'url(' + require('@/../resources/background.jpg') + ')  no-repeat',
      backgroundSize: '410px',
    }"
  >
    <div class="mainPage">
      <div class="container">
        <img id="logo" src="../../resources/logo.jpg" />
        <RecipePreviewList
          class="RandomRecipes center"
          title="Try a new recipe"
        />
        <div class="bottom-half">
          <router-link v-if="!$root.store.username" to="/login" tag="button"
            >You need to Login to vue this</router-link
          >
          <RecipePreviewList
            title="Last Viewed Recipes"
            :class="{
              RandomRecipes: true,
              blur: !$root.store.username,
              center: true,
            }"
            disabled
          ></RecipePreviewList>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  mounted() {
    console.log("main page mounted");
  },
  components: {
    RecipePreviewList,
  },
};
</script>

<style lang="scss" scoped>
container {
  padding: 20px;
  font-family: Helvetica;
}

.RandomRecipes {
  margin: auto;
}
.blur {
  -webkit-filter: blur(5px);
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

.bottom-half {
  margin-top: 35px;
}
</style>
