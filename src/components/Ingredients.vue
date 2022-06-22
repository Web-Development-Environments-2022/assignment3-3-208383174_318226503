<template>
  <div>
    <h1>Ingredients:</h1>
    <ul>
      <li v-for="(r, index) in ingredients" :key="index + '_' + r.id">
        {{ r.originalName }},
        {{ r.amount }}
        {{ r.unit }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Ingredients",
  props: {
    ingredients: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
h1 {
  font-size: 25px;
}

ul {
  list-style-image: url("../../resources/peach.png");
}

li:hover {
  list-style-image: url("../../resources/peach-hover.png");
}

li:before {
  font-size: 100px;
}
</style>
