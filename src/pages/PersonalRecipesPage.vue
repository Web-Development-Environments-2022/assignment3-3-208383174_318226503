<template>
  <div
    class="PersonalPage"
    :style="{
      background:
        'url(' + require('@/../resources/background.jpg') + ')  no-repeat',
      backgroundSize: '410px',
    }"
  >
    <div class="PersonalPage">
      <div class="container">
        <PersonalRecipePreviewList
          class="PersonalRecipes center"
          title="Your Personal Recipes"
        />
        <router-link v-if="!$root.store.username" to="/login" tag="button"
          >You need to Login to view this</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
import PersonalRecipePreviewList from "../components/PersonalRecipePreviewList";
export default {
  mounted() {
    console.log("Personal page mounted");
  },
  components: {
    PersonalRecipePreviewList,
  },
};
</script>

<style lang="scss" scoped>
container {
  padding: 20px;
  font-family: Helvetica;
}

.PersonalRecipes {
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
