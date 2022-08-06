<template>
  <div>
    <h1>Your Upcomming Meal</h1>
    <div>
        <table class="table">
            <thead>
            <th>Number</th>
            <th>Recipe</th>
            <th>Progress</th>
            <th>Change Order</th>
            <th>Delete</th>
            </thead>

            <tr v-for="r in todo_recipes" :key="r.order">
                <td>{{r.order}}</td>
                <td> <RecipePreview class="recipePreview" :recipe="r.recipe_preview"></RecipePreview></td>
                <td>TODO</td>
                <td>
                    <b-button>Down</b-button>
                    <b-button>Up</b-button>
                </td>
                <td><b-button>Delete</b-button></td>
            </tr>
        </table>

    </div>
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
export default {
  name: "MealPlanning",
  data() {
    return {
        todo_recipes: [],
    };
  },
  mounted() {
    this.getUpcommingMeal();
  },
  components: {
    RecipePreview,
  },
  methods:{
    async getUpcommingMeal(){
        const DOMAIN_PATH = "http://localhost:3000";
        try{
            let res = await this.axios
                .create({ withCredentials: true })
                .get(DOMAIN_PATH + "/users/getUpcommingMeal", {
                    withCredentials: true,
                });
            this.todo_recipes= res.data;
            console.log(this.todo_recipes);
        }
        catch(error){
            console.log("ERROR !");
        }
    }
  }
};
</script>