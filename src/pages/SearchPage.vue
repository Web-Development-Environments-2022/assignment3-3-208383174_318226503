
<template>
  <div class="container">
    <h1 class="title">Search Page</h1>

    <b-navbar type="light" variant="light">
      <b-nav-form>
        <b-row class="my-1">
          <b-col md="9">
            <b-form-input
              id="search"
              placeholder="Search..."
              v-model="$v.form.search.$model"
              label-for="search"
              type="text"
              :state="validateState('search')"
              size="lg"
            ></b-form-input>
            <b-form-invalid-feedback v-if="$v.form.search.length"></b-form-invalid-feedback>
          </b-col>

          <b-col>
          <b-dropdown id ="numSearchResult" class="m-2" :text="numberOfResults_text">
          <b-dropdown-item @click="numSearchResult('5')">5</b-dropdown-item>
          <b-dropdown-item @click="numSearchResult('10')">10</b-dropdown-item>
          <b-dropdown-item @click="numSearchResult('15')">15</b-dropdown-item>
        </b-dropdown>
          </b-col>
        </b-row>



      </b-nav-form>
              
    <b-button variant="outline-success" class="my-2 my-sm-0" type="submit" @click="onSearch">Search</b-button>
    </b-navbar>
        <b-row cols="3">
          <b-col v-for="item in results" :key="item.previewInfo.id">
            <SearchResultsPreview class="searchResultsPreview" :recipe="item" />
          </b-col>
        </b-row>
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview";
import { required, minLength, alpha } from "vuelidate/lib/validators";
import cuisines from "../assets/cuisines";
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
      cuisines: [{ value: null, text: "", disabled: true }],
      //diets: ,
      //intolerances: ,
      //user_cusine: "",
      //user_diet: "",
      //user_intolerances: "",
      noResults: false,
      numberOfResults_text:"Number of search results"
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
    // last search
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
        const response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          // this.$root.store.server_domain + "/Register",
          DOMAIN_PATH + "/recipes/search?term="+this.userSearchTerm + "&numOfResults="+this.numberOfSearchResults,
        );
        console.log(response);
        const recipes = response.data;
        this.results = [];
        this.results.push(...recipes);
      } catch (err) {
        console.log(err.response);
      }
    },
    onSearch() {
      console.log("search method called");
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
      this.numberOfResults_text = num
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

</style>