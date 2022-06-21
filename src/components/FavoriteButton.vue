<template>
  <div>
    <b-icon
      v-if="recipe.isFavorite === false"
      class="heart_icon"
      icon="heart"
      font-scale="2"
    ></b-icon>

    <b-icon
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
  methods: {},
};
</script>

<style scoped></style>
