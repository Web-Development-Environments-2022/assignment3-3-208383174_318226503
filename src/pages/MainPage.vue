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
          <h4>Last watched recipes</h4>
          <router-link
            id="login"
            v-if="!$root.store.username"
            to="/login"
            tag="button"
            >You need to Login to vue this</router-link
          >
          <div v-if="$root.store.username">
            <RecipePreviewListLastSeen
              :class="{
                center: true,
              }"
              disabled
            ></RecipePreviewListLastSeen>
          </div>
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

h4 {
  font-size: 27px;
  font-family: Andale Mono, monospace;
  margin-bottom: 10px;
  text-align: center;
}

#login {
  display: flex;
  margin: auto;
  background-color: #cdf0c4;
  padding: 4px 15px 4px 15px;
  border-radius: 36px;
  width: 30%;
  place-content: center;
  font-size: 18px;
  border: 1px solid white;
}
</style>
