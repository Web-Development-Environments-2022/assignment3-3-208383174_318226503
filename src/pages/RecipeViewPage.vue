<template>
  <b-card no-body class="overflow-hidden" style="max-width: 80%; height:800px">
    <div class="container">
      <div v-if="recipe">
        <img id="recipe-image" :src="recipe.image" class="center" />
        <FavoriteButton id="favoriteButton" :recipe="recipe" />
      </div>
      <div class="recipe-body">
        <h2>{{ recipe.title }}</h2>
        <div id="recipe-info">
          <span
            ><div class="info" id="minutes">
              <div class="number">{{ recipe.readyInMinutes }}</div>
              <div class="description">minutes</div>
            </div>
            <div class="info">
              <div class="number">{{ recipe.popularity }}</div>
              <div class="description">likes</div>
            </div>
          </span>
        </div>
        <div class="icons">
          <img
            id="vegetarian"
            v-if="recipe.vegetarian === true"
            src="../../resources/vegeterian.png"
            height="50px"
            width="50px"
            title="vegetarian"
          />
          <img
            id="vegan"
            v-if="recipe.vegan === true"
            src="../../resources/vegan.png"
            height="50px"
            width="50px"
            title="vegan"
          />
          <img
            id="glutenFree"
            v-if="recipe.glutenFree === true"
            src="../../resources/gluten-free2.png"
            height="50px"
            width="50px"
            title="gluten free"
          />
          <img
            id="viewed"
            v-if="recipe.isViewed === true"
            src="../../resources/viewed.png"
            height="50px"
            width="50px"
            title="you have viewed this recipe"
          />
          <img
            id="viewed"
            v-else-if="recipe.isViewed === false"
            src="../../resources/new.png"
            height="50px"
            width="50px"
            title="this is the first time you're viewing this recipe"
          />
        </div>
      </div>
    </div>
  </b-card>
</template>

<script>
import FavoriteButton from "../components/FavoriteButton.vue";

export default {
  components: {
    FavoriteButton,
  },
  data() {
    return {
      recipe: null,
    };
  },
  async created() {
    console.log("getting recipe");
    try {
      let response;

      try {
        console.log("recipe id " + this.$route.params.recipeId);
        const DOMAIN_PATH = "http://localhost:3000";

        response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/recipes/info",
          DOMAIN_PATH + "/recipes/" + this.$route.params.recipeId
        );

        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      let {
        analyzedInstructions,
        extendedIngredients,
        servingSize,
      } = response.data;

      let {
        popularity,
        readyInMinutes,
        image,
        title,
        vegan,
        vegetarian,
        glutenFree,
        isFavorite,
        isViewed,
      } = response.data.previewInfo;

      let _instructions = analyzedInstructions
        .map((fstep) => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        _instructions,
        analyzedInstructions,
        extendedIngredients,
        popularity,
        readyInMinutes,
        image,
        title,
        vegan,
        vegetarian,
        glutenFree,
        isFavorite,
        isViewed,
        servingSize,
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style scoped>
#recipe-image {
  height: 380px;
  /* width: auto; */
  margin-top: 20px;
  border-radius: 4%;
  position: absolute;
  right: 80px;
}

.container {
  margin-top: 20px;
}

.overflow-hidden {
  margin: auto;
}

.recipe-body {
  width: 600px;
  margin-top: 50px;
  position: absolute;
  left: 200px;
}

.icons {
  text-align: center;
  margin-top: 50px;
}

.icons img {
  margin: 0 22px 0 22px;
}

h2 {
  color: rgb(207, 97, 50);
  font-size: 35px;
  margin-left: 0;
  text-align: center;
}

#recipe-info {
  padding-top: 40px;
  position: relative;
  margin: auto;
  left: 100px;
}

.info {
  margin: 0 20px 0 20px;
  text-align: center;
}

#minutes {
  border-right: 1px solid #da541a;
  padding-right: 30px;
}

.number {
  font-size: 32px;
}

.description {
  font-size: 20px;
}

#recipe-info span {
  font-size: 23px;
  display: flex;
  margin-left: 85px;
}

#favoriteButton {
  position: absolute;
  right: 600px;
  top: 375px;
  font-size: 21px;
}
</style>
