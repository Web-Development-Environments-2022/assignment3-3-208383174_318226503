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
        <div>
          <RecipePreviewListRandom
            :key="componentKey"
            class="RandomRecipes center"
            title="Explore this recipes"
          />
        </div>
        <div class="bottom-half">
          <router-link v-if="!$root.store.username" to="/login" tag="button"
            >You need to Login to vue this</router-link
          >
          <RecipePreviewListLastSeen
            title="Last watched recipes"
            :class="{
              center: true,
            }"
            disabled
          ></RecipePreviewListLastSeen>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RecipePreviewListRandom from "../components/RecipePreviewListRandom.vue";
import RecipePreviewListLastSeen from "../components/RecipePreviewListLastSeen.vue";
export default {
  data() {
    return {};
  },
  mounted() {
    console.log("main page mounted");
  },
  components: {
    RecipePreviewListLastSeen,
    RecipePreviewListRandom,
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

.random-button {
  margin: auto;
  display: flow-root;
  margin-top: 20px;
  padding: 6px 13px 6px 13px;
  font-size: 19px;
  border: 1px solid white;
  border-radius: 6px;
  background-color: #e69d3c;
  color: white;
  font-family: sans-serif;
}

.bottom-half {
  margin-top: 17px;
}
</style>
