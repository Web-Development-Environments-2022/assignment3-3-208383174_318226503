
<template>
  <div class="container">
    <h1 class="title">Search Page</h1>

    <!-- <b-navbar type="light" variant="light"> -->
      <b-nav-form>
        <b-container>
        <b-row >
          <b-col>
            <b-form-input
              id="search"
              placeholder="Search..."
              v-model="$v.form.search.$model"
              label-for="search"
              type="text"
              :state="validateState('search')"
              size="lg"
              style="width: 800px;"
            ></b-form-input>
          </b-col>
          <b-form-invalid-feedback v-if="$v.form.search.length"></b-form-invalid-feedback>
        <b-col>
          <b-button variant="outline-success" type="submit" @click="onSearch" size="lg">Search</b-button>
        </b-col>
          </b-row>
          <b-row>
          <b-col>
          <b-dropdown id ="numSearchResult" class="m-2" :text="numberOfResults_text">
          <b-dropdown-item @click="numSearchResult('5')">5</b-dropdown-item>
          <b-dropdown-item @click="numSearchResult('10')">10</b-dropdown-item>
          <b-dropdown-item @click="numSearchResult('15')">15</b-dropdown-item>
          </b-dropdown>
          </b-col>
          <!-- <b-col>
            <b-form-select
            id="cuisine"
            :options="cuisines"
            v-model="selectedCuisine"
            v-on:change="setCuisine"
          ></b-form-select>
          </b-col> -->

          <!--cuisine dropdown box-->
          <b-col>
            <b-dropdown id ="cuisine" class="m-2" :text="selectedCuisine">
            <b-dropdown-item-button
                      v-for="c in cuisines"
                      :key="c.text"
                      @click.prevent="setCuisine(c)"
                      >{{ c }}</b-dropdown-item-button
                    >
            </b-dropdown>
          </b-col>
          <!--diet dropdown box-->
          <b-col>
            <b-dropdown id ="diet" class="m-2" :text="selectedDiet">
            <b-dropdown-item-button
                      v-for="c in diets"
                      :key="c.text"
                      @click.prevent="setDiet(c)"
                      >{{ c }}</b-dropdown-item-button
                    >
            </b-dropdown>
          </b-col>
          <!--intolerance dropdown box-->
          <b-col>
            <b-dropdown id ="intolerance" class="m-2" :text="selectedIntolerance">
            <b-dropdown-item-button
                      v-for="c in intolerances"
                      :key="c.text"
                      @click.prevent="setIntolerance(c)"
                      >{{ c }}</b-dropdown-item-button
                    >
            </b-dropdown>
          </b-col>
          </b-row>
        </b-container>
      </b-nav-form>
    <!-- </b-navbar> -->
    <b-row  v-if="this.isEmpty==0" >
      <b-form-group  label="Sort by:">
        <b-form-radio
        value="cooking_duration"
        name="sortOpt"
        @change.native="sortByCookingDuration($event)"
        >Cooking Duration</b-form-radio>
        <b-form-radio
        value="popularity"
        name="sortOpt"
        @change.native="sortByPopularity($event)"
        >Popularity</b-form-radio>
    </b-form-group>
      </b-row>
        <b-row v-if="this.isEmpty==0" cols="3">
          <b-col v-for="item in results" :key="item.previewInfo.id">
            <SearchResultsPreview class="searchResultsPreview" :recipe="item" />
          </b-col>
          <!-- <h1 v-if=this.isEmpty>Oops! Nothing matches your search. <br> please try again</h1> -->
          <b-alert v-if="this.isEmpty==1" show variant="danger"><a class="alert-link">Oops! Nothing matches your search. 
            <br> Please try again.</a></b-alert>
        </b-row>
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview";
import { required, minLength, alpha } from "vuelidate/lib/validators";
import cuisines from "../assets/cuisines";
import diets from "../assets/diets";
import intolerances from "../assets/intolerances";
import SearchResultsPreview from "../components/SearchResultsPreview.vue"; 
export default {
    name: 'searchResult',
    data () {
      return {
        form: {
        search: ""
      },
      search: "",
      numberOfResults: 5, //default
      hasResult: false,
      results: [],
      selectedCuisine:"Cuisine",
      selectedDiet: "Diet",
      selectedIntolerance: "Intolerance",
      cuisines: [],
      diets: [],
      intolerances: [],
      cusine: "",
      diet: "",
      intolerance: "",
      numberOfResults_text:"Number of search results",
      isEmpty: 5,
      };
    },
    validations: {
    form: {
      search: {
        required,
        length: u => minLength(1)(u),
        alpha
      }
    },
  },
  mounted() {
    // TODO: last search
    console.log("mounted");
    this.cuisines.push(...cuisines);
    this.diets.push(...diets);
    this.intolerances.push(...intolerances);
    console.log("localStorage.username: "+localStorage.username );
    // console.log(req.session.user_id);
  },
  components: {
    SearchResultsPreview,
},

  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      const DOMAIN_PATH = "http://localhost:3000";
      console.log("search function");
      try {
        let path_to_exe = DOMAIN_PATH + "/recipes/search?term="+this.userSearchTerm + "&numOfResults="+this.numberOfSearchResults;
        if ( this.selectedCuisine != null && this.selectedCuisine != "Cuisine"){
          path_to_exe += "&cuisine="+this.selectedCuisine;
        }
        if(this.selectedDiet != null && this.selectedDiet != "Diet"){
          path_to_exe += "&diet="+this.selectedDiet;
        }
        if(this.selectedIntolerance != null && this.selectedIntolerance != "Intolerance"){
          path_to_exe += "&intolerance="+this.selectedIntolerance;
        }
        const response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          // this.$root.store.server_domain + "/Register",
          path_to_exe, { withCredentials: true }
        );
        console.log(response);
        console.log(response.status)
        if(response.status==204 && response.statusText=="No Content"){
          this.isEmpty = 1;
        }
        else if(response.status==200){
          this.isEmpty = 0;
        }
        const recipes = response.data;
        this.results = [];
        this.results.push(...recipes);
      } catch (err) {
        console.log(err.response);
      }
    },
    onSearch() {
      console.log("search method called");
      // console.log(this.selectedCuisine);
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      console.log("search method go");
      this.userSearchTerm = this.form.search;
      this.Search();
    },
    numSearchResult(num){
      this.numberOfSearchResults = num.toString();
      this.numberOfResults_text = num;
    },
    setCuisine(cType){
      console.log(cType);
      if (cType!= "All Cuisines" || cType!= "Cuisine"){
        this.selectedCuisine = cType;
        // console.log(cType+ "!= All Cuisines");
      }
      else{
        this.selectedCuisine = null;
      }
    },
    setDiet(dType){
      console.log(dType);
      if (dType!= "All Diets" || dType != "Diet"){
        this.selectedDiet = dType;
        // console.log(dType+ "!= All Diets");
      }
      else{
        this.selectedDiet = null;
      }
    },
    setIntolerance(iType){
      console.log(iType);
      if (iType!= "No Intolerances" || iType != "Intolerance"){
        this.selectedIntolerance = iType;
        // console.log(iType+ "!= No Intolerances");
      }
      else{
        this.selectedIntolerance = null;
      }
    },
    sortByCookingDuration: function(){
      this.isEmpty= 1; //to stop show of recipes
      this.results.sort(function(x,y){return x.previewInfo.readyInMinutes-y.previewInfo.readyInMinutes;});
      this.isEmpty = 0; //show after sort
    },
    sortByPopularity: function(){
      this.isEmpty= 1; //to stop show of recipes
      this.results.sort(function(x,y){return y.previewInfo.popularity-x.previewInfo.popularity;});
      this.isEmpty = 0; //show after sort
    }
  },
};
</script>

<style lang="scss" scoped>
container {
  padding: 20px;
  font-family: Helvetica;
}

.SearchRecipes {
  // margin: 10px 0 10px;
  margin: auto;
}

.alert {
   width:900px;    
}

</style>