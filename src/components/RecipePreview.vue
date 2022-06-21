<template>
  <div>
    <div class="card" style="width: 20rem; height: 25rem;">
      <router-link
        :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
        id="recipe-preview"
      >
        <div class="recipe-body">
          <img v-if="image_load" :src="recipe.image" class="card-img-top" />
        </div>
        <h5 class="card-title">
          {{ recipe.title }}
        </h5>
      </router-link>
      <div class="card-body">
        <ul class="list-group list-group-flush">
          <p id="recipe_info">
            {{ recipe.readyInMinutes }} minutes | {{ recipe.popularity }} likes
          </p>
          <p id="viewed_recipe" v-if="recipe.isViewed === true">
            you already viewed this recipe
          </p>
          <div class="food_icons">
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
            <img
              id="vegetarian"
              v-if="recipe.vegetarian === true"
              src="../../resources/vegeterian.png"
              height="30px"
              width="30px"
              title="vegetarian"
            />
            <img
              id="vegan"
              v-if="recipe.vegan === true"
              src="../../resources/vegan.png"
              height="30px"
              width="30px"
              title="vegan"
            />
            <img
              id="glutenFree"
              v-if="recipe.glutenFree === true"
              src="../../resources/gluten-free.png"
              height="30px"
              width="30px"
              title="gluten free"
            />
          </div>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    console.log("recipe preview mounted");
    this.axios.get(this.recipe.image).then((i) => {
      this.image_load = true;
    });
  },
  data() {
    return {
      image_load: false,
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
#recipe-preview {
  display: inline-block;
  width: 100%;
  height: auto;
  position: relative;
  text-decoration: none;
}

.card-img-top:hover {
  opacity: 0.85;
}

.food_icons {
  position: absolute;
  top: 73px;
}

.food_icons img {
  margin-right: 10px;
  position: relative;
}

.food_icons .heart_icon {
  position: absolute;
  left: 257px;
}

#vegetarian {
  position: relative;
  bottom: 0px;
}

.card {
  color: black;
}

.card-body {
  padding-top: 4px;
  position: absolute;
  top: 285px;
}

.card .card-title {
  margin-top: 0px;
  margin-bottom: 10px;
  overflow: hidden;
  display: -webkit-box;
  max-width: 20rem;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  padding: 0.6rem 1.25rem 0 1.25rem;
  color: black;
}

.card .card-title:hover {
  text-decoration: none;
  color: rgb(100, 100, 100);
}

.recipe-body {
  position: relative;
  width: 100%;
}

#recipe_info {
  margin-bottom: 2px;
  margin-top: 0px;
}

#viewed_recipe {
  margin-top: 3px;
  color: rgb(165, 161, 161);
}

.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}
</style>
