<template>
  <div>
    <b-modal id="modal-1" title="Create Your New Recipe">
      <div>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <!-- title -->
          <b-form-group id="title" label="Title:" label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="form.title"
              type="text"
              placeholder="Enter your recipe title"
              required
            ></b-form-input>
          </b-form-group>

          <!-- image -->
          <b-form-group
            id="input-group-2"
            label="Image url:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="form.image"
              placeholder="Enter your recipe's image url"
              required
            ></b-form-input>
          </b-form-group>

          <!-- readyInMinutes -->
          <b-form-group
            id="input-group-3"
            label="Making time:"
            label-for="input-3"
          >
            <b-form-input
              id="input-3"
              type="number"
              v-model="form.readyInMinutes"
              placeholder="How many minutes does it takes to make the recipe"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            label="select if the following maches your recipe"
            id="input-group-4"
            v-slot="{ ariaDescribedby }"
          >
            <b-form-checkbox-group
              v-model="form.checked"
              id="checkboxes-4"
              :aria-describedby="ariaDescribedby"
            >
              <b-form-checkbox value="vegan">Vegan</b-form-checkbox>
              <b-form-checkbox value="vegeterian">Vegeterian</b-form-checkbox>
              <b-form-checkbox value="gluten-free">Gluten Free</b-form-checkbox>
            </b-form-checkbox-group>

            <!-- ingredients -->
            <div class="ingredients">
              <h5>add the ingredients</h5>
              <b-form inline @submit.prevent="addIngredient">
                <label class="sr-only" for="inline-form-input-name">Name</label>
                <b-form-input
                  id="ingredient-name"
                  v-model="form.ingredientName"
                  class="mb-2 mr-sm-2 mb-sm-0"
                  placeholder="Name"
                ></b-form-input>

                <label class="sr-only" for="inline-form-input-name"
                  >Amount</label
                >
                <b-form-input
                  id="amount"
                  v-model="form.amount"
                  class="mb-2 mr-sm-2 mb-sm-0"
                  placeholder="Amount"
                  type="number"
                ></b-form-input>
                <label class="sr-only" for="inline-form-input-name">Unit</label>
                <b-form-input
                  id="unit"
                  v-model="form.unit"
                  class="mb-2 mr-sm-2 mb-sm-0"
                  placeholder="Unit"
                  type="number"
                ></b-form-input>

                <b-button type="addIngredient" variant="primary">Add</b-button>
                <div v-if="showIngridentsMeesage === true">
                  <div>
                    <b-alert show dismissible>
                      Successfully added {{ IngridentsMeesage }} to the recipe
                    </b-alert>
                  </div>
                </div>
              </b-form>
            </div>

            <!-- instructions -->
            <div class="instructions">
              <h5>add the instructions</h5>
              <b-form inline @submit.prevent="addInstructions">
                <label class="sr-only" for="inline-form-input-name"
                  >instruction</label
                >
                <b-form-input
                  placeholder="Instruction Step"
                  id="instruction-name"
                  value="instruction"
                  v-model="form.step"
                  class="mb-2 mr-sm-2 mb-sm-0"
                ></b-form-input>

                <b-button type="addInstructions" variant="primary"
                  >Add</b-button
                >
                <div v-if="showInstructionMeesage === true">
                  <div>
                    <b-alert show dismissible>
                      Successfully added number
                      {{ instructionsArray.length }} step to the recipe
                    </b-alert>
                  </div>
                </div>
              </b-form>
            </div>
          </b-form-group>

          <b-button type="submit" variant="primary">Submit</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </b-form>
      </div>
      <template #modal-footer="{ cancel }">
        <b-button size="sm" @click="cancel()">
          close
        </b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "NewRecipe",
  created() {
    this.ingridents = [];
    this.instructionsArray = [];
    this.showIngridentsMeesage = false;
    this.showInstructionMeesage = false;
    this.IngridentsMeesage = "";
  },
  data() {
    return {
      form: {
        title: "",
        image: "",
        servingSize: "",
        first_time: "",
        readyInMinutes: null,
        vegan: null,
        vegetarian: null,
        glutenFree: null,
        extendedIngredients: [],
        analyzedInstructions: null,
        checked: [],
        ingredientName: "",
        amount: null,
        unit: "",
        step: "",
      },

      show: true,
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.food = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    addIngredient(event) {
      console.log("you sumbitted");
      event.preventDefault();
      this.showIngridentsMeesage = true;
      this.IngridentsMeesage = this.form.ingredientName;

      console.log(this.ingridents);
      this.ingridents.push({
        ingredientName: this.form.ingredientName,
        amount: this.form.amount,
        unit: this.form.unit,
      });
      console.log(this.ingridents);
      this.form = {
        ingredientName: "",
        amount: null,
        unit: "",
      };
    },
    addInstructions(event) {
      console.log(this.instructionsArray.length);
      event.preventDefault();
      this.showInstructionMeesage = true;

      this.instructionsArray.push({
        number: this.instructionsArray.length + 1,
        step: this.form.step,
      });
      console.log(this.instructionsArray);
      console.log(this.instructionsArray.length);
      this.form = {
        step: "",
      };
    },
  },
};
</script>

<style scope>
h5 {
  font-size: 1rem;
  font-weight: normal;
}

.ingredients {
  margin-top: 18px;
}
</style>

<style>
#modal-1 .modal-dialog {
  max-width: 800px;
}
</style>
