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
        </div>
          <div class="info">
            <br>
            <br>
            <span
                ><div class="info" id="owner">
                <div class="description">Recipe's owner: {{ recipe.owner }}</div>
                </div>
                <div class="info"  id="occasion">
                <div class="description">{{ recipe.occasion }}</div>
                </div>
          </span>
          </div>
      </div>
      <div v-if="recipe" class="recipe-img-container">
        <img id="recipe-image" :src="recipe.image" class="center" />
      </div>
    </div>
    <div class="divider div-transparent"></div>
    <div class="recipe-full">
      <div id="instructions">
        <FamilyRecipeInstructions :instructions="recipe.instructions" />
      </div>
      <div id="ingredients">
        <FamilyRecipeIngredients 
        :ingredient_amount="recipe.ingredient_amount"
        :ingredient_sizing="recipe.ingredient_sizing"
        :ingredients="recipe.ingredients" />
      </div>
    </div>
  </b-card>
</template>

<script>
import FamilyRecipeIngredients from "../components/FamilyRecipeIngredients.vue";
import FamilyRecipeInstructions from "../components/FamilyRecipeInstructions.vue";

export default {
  components: {
    FamilyRecipeIngredients,
    FamilyRecipeInstructions,
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
        DOMAIN_PATH = "http://localhost:3000/users/myFamilyRecipes";
        response = await this.axios.get(
          DOMAIN_PATH,
          { withCredentials: true }
        );
        if (response.status !== 200) this.$router.replace("/NotFound");
        console.log("response.data is : ");
        // console.log(response.data);
        // console.log(response.data[0]);
        // console.log(response.data.recipe_id);
    let {
        recipe_id,
        user_id,
        title,
        image,
        occasion,
        owner,
        readyInMinutes,
        vegan,
        vegetarian,
        glutenFree,
        servingSize,
        ingredient_amount,
        ingredient_sizing,
        ingredients,
        instructions
    } = response.data[0];


    let _recipe = {
        recipe_id,
        user_id,
        title,
        image,
        occasion,
        owner,
        readyInMinutes,
        // vegan,
        // vegetarian,
        // glutenFree,
        servingSize,
        ingredient_amount,
        ingredient_sizing,
        ingredients,
        instructions
    };


    if (this.readyInMinutes === null || this.readyInMinutes === undefined) {
    console.log("true 1");
    this.readyInMinutes = "0";
    }

    if (this.servingSize === null || this.servingSize === undefined) {
    console.log("true 2");
    this.servingSize = 0;
    }

    console.log(readyInMinutes);
    console.log(servingSize);

    this.recipe = _recipe;

    } catch (error) {
    console.log("error.response.status", error.response.status);
    this.$router.replace("/NotFound");
    return;
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
.makeNowButton{
  font-size: 20px;
  background-color: rgb(235, 222, 195);
  border-color: rgb(255, 195, 127);
}
#ingredients {
  float: left;
}

#instructions {
  float: left;
  width: 65%;
  padding: 0 50px 0 50px;
}
#MakingRecipeSteps_div{
  float: left;
  width: 68%;
  padding: 0 50px 0 50px;
}
</style>
