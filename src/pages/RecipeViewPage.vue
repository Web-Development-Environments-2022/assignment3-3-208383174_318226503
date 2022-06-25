<template>
  <b-card
    no-body
    class="overflow-hidden"
    style="max-width: 1200px; min-width: 1000px;"
  >
    <div class="container">
      <div class="recipe-body">
        <h2>{{ recipe.title }}</h2>
        <div id="recipe-info">
          <span
            ><div class="info" id="minutes">
              <div class="number">{{ recipe.readyInMinutes }}</div>
              <div class="description">minutes</div>
            </div>
            <div class="info" id="likes">
              <div class="number">{{ recipe.popularity }}</div>
              <div class="description">likes</div>
            </div>
            <div class="info">
              <div class="number">{{ recipe.servingSize }}</div>
              <div class="description">Dishes</div>
            </div>
          </span>
        </div>
        <div class="icons">
          <img
            id="vegetarian"
            v-if="recipe.vegetarian === true"
            src="../../resources/vegeterian.png"
            title="vegetarian"
          />
          <img
            id="vegan"
            v-if="recipe.vegan === true"
            src="../../resources/vegan.png"
            title="vegan"
          />
          <img
            id="glutenFree"
            v-if="recipe.glutenFree === true"
            src="../../resources/gluten-free2.png"
            title="gluten free"
          />
          <img
            id="viewed"
            v-if="recipe.isViewed === true"
            src="../../resources/viewed.png"
            title="you have viewed this recipe"
          />
          <img
            id="viewed"
            v-else-if="recipe.isViewed === false"
            src="../../resources/new.png"
            title="this is the first time you're viewing this recipe"
          />
        </div>
      </div>
      <div v-if="recipe" class="recipe-img-container">
        <img id="recipe-image" :src="recipe.image" class="center" />
        <FavoriteButton id="favoriteButton" :recipe="recipe.previewInfo" />
      </div>
    </div>
    <div class="divider div-transparent"></div>
    <div class="recipe-full">
      <div id="instructions">
        <Instructions :instructions="recipe._instructions" />
      </div>
      <div id="ingredients">
        <Ingredients :ingredients="recipe.extendedIngredients" />
      </div>
    </div>
  </b-card>
</template>

<script>
import FavoriteButton from "../components/FavoriteButton.vue";
import Ingredients from "../components/Ingredients.vue";
import Instructions from "../components/Instructions.vue";

export default {
  components: {
    FavoriteButton,
    Ingredients,
    Instructions,
  },
  data() {
    return {
      recipe: null,
    };
  },
  async mounted() {
    let DOMAIN_PATH;

    try {
      let response;
      console.log(this.$route);

      if (this.$route.params.isPersonal) {
        console.log("personal");
        DOMAIN_PATH = "http://localhost:3000/users/personal/";
      } else {
        console.log("not personal");
        DOMAIN_PATH = "http://localhost:3000/recipes/";
      }

      try {
        response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/recipes/info",
          DOMAIN_PATH + this.$route.params.recipeId,
          { withCredentials: true }
        );

        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      console.log(response);

      let {
        analyzedInstructions,
        extendedIngredients,
        servingSize,
        previewInfo,
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
        previewInfo,
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style scoped>
.recipe-img-container {
  width: 40%;
  float: right;
  position: relative;
  margin-top: 15px;
}
#recipe-image {
  border-radius: 4%;
  width: 100%;
}
.container {
  margin-top: 20px;
}

.overflow-hidden {
  margin: auto;
}

.recipe-body {
  width: 57%;
  margin-top: 40px;
  display: inline-block;
}

.icons {
  text-align: center;
  margin-top: 50px;
}

.icons img {
  margin: 0 22px 0 22px;
  width: 50px;
}

h2 {
  color: rgb(225, 118, 11);
  font-size: 35px;
  margin-left: 0;
  text-align: center;
  font-family: FreeMono, monospace;
}

#recipe-info {
  padding-top: 30px;
  position: relative;
  margin: auto;
  left: 100px;
}

.info {
  margin: 0 20px 0 20px;
  text-align: center;
}

#minutes,
#likes {
  border-right: 1px solid rgb(225, 118, 11);
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
  margin-left: 20px;
}

#favoriteButton {
  position: absolute;
  bottom: 2px;
  left: 8px;
  font-size: 22.5px;
}

.divider {
  position: relative;
  margin-top: 20px;
  margin-bottom: 10px;
  height: 1px;
}

.div-transparent:before {
  content: "";
  position: absolute;
  top: 0;
  left: 5%;
  right: 5%;
  width: 90%;
  height: 1px;
  background-image: linear-gradient(
    to right,
    transparent,
    rgb(174, 168, 165),
    transparent
  );
}

#ingredients {
  float: left;
}

#instructions {
  float: left;
  width: 65%;
  padding: 0 50px 0 50px;
}
</style>
