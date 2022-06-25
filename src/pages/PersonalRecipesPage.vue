<template>
  <div
    class="PersonalPage"
    :style="{
      background:
        'url(' + require('@/../resources/background.jpg') + ')  no-repeat',
      backgroundSize: '410px',
    }"
  >
    <div class="personal-page">
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
.container {
  padding: 2px;
  min-height: 800px;
  font-family: Helvetica;
}

.personal-page {
  margin-top: 30px;
  margin: auto;
}
</style>
