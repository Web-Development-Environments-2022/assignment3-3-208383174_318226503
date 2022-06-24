<template>
  <div>
    <b-modal id="modal-1" title="Create Your New Recipe">
      <div>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <!-- title -->
          <b-form-group id="title" label="Title:" label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="$v.form.title.$model"
              type="text"
              placeholder="Enter your recipe title"
              :state="validateState('title')"
              required
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.title.required">
              title is required
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-else-if="!$v.form.title.length">
              title length should be between up to 100 characters long
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-if="!$v.form.title.alpha">
              title should include letters only
            </b-form-invalid-feedback>
          </b-form-group>

          <!-- image -->
          <b-form-group
            id="input-group-2"
            label="Image url:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="$v.form.image.$model"
              placeholder="Enter your recipe's image url"
              :state="validateState('image')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.image.url">
              please enter a valid url
            </b-form-invalid-feedback>
          </b-form-group>

          <!-- readyInMinutes && ServingSize -->
          <div>
            <b-form inline>
              <label class="mr-sm-2" for="inline-form-custom-select-pref"
                >Making Time</label
              >
              <b-form-input
                v-model="$v.form.readyInMinutes.$model"
                id="inline-form-input-readyTime"
                class="mb-2 mr-sm-2 mb-sm-0"
                type="number"
                :state="validateState('readyInMinutes')"
              ></b-form-input>
              <label class="mr-sm-2" for="inline-form-custom-select-pref"
                >Making Time</label
              >
              <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.minValue">
                Not Good
              </b-form-invalid-feedback>

              <b-form-input
                v-model="$v.form.servingSize.$model"
                id="inline-form-input-username"
                type="number"
                :state="validateState('servingSize')"
              ></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.servingSize.minValue">
                Not Good
              </b-form-invalid-feedback>
            </b-form>
          </div>

          <!-- nutritious -->
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
              <b-form-checkbox
                v-model="form.nutritious"
                value="vegan"
                unchecked-value="not_vegan"
                >Vegan</b-form-checkbox
              >
              <b-form-checkbox
                value="vegetarian"
                unchecked-value="not_vegetarian"
                >Vegeterian</b-form-checkbox
              >
              <b-form-checkbox
                value="gluten-free"
                unchecked-value="not_gluten-free"
                >Gluten Free</b-form-checkbox
              >
            </b-form-checkbox-group>

            <!-- ingredients -->
            <div class="ingredients">
              <h5 class="input-title">add the ingredients</h5>
              <b-form inline @submit.prevent="addIngredient">
                <label class="sr-only" for="inline-form-input-name">Name</label>
                <b-form-input
                  id="ingredient-name"
                  v-model="$v.form.ingredientName.$model"
                  class="mb-2 mr-sm-2 mb-sm-0"
                  placeholder="Name"
                  :state="validateState('ingredientName')"
                ></b-form-input>

                <label class="sr-only" for="inline-form-input-name"
                  >Amount</label
                >
                <b-form-input
                  id="amount"
                  v-model="$v.form.amount.$model"
                  class="mb-2 mr-sm-2 mb-sm-0"
                  placeholder="Amount"
                  type="number"
                  :state="validateState('amount')"
                ></b-form-input>
                <label class="sr-only" for="inline-form-input-name">Unit</label>
                <b-form-input
                  id="unit"
                  v-model="$v.form.unit.$model"
                  class="mb-2 mr-sm-2 mb-sm-0"
                  placeholder="Unit"
                  type="number"
                  :state="validateState('unit')"
                ></b-form-input>

                <b-button type="submit" variant="primary">Add</b-button>
                <div v-if="showIngridentsMeesage === true">
                  <div>
                    <b-alert show dismissible>
                      Successfully added {{ IngridentsMeesage }} to the recipe
                    </b-alert>
                  </div>
                </div>

                <b-form-invalid-feedback v-if="!$v.form.ingredientName.length">
                  ingredient name should be up to 100 characters long
                </b-form-invalid-feedback>
                <b-form-invalid-feedback
                  v-else-if="!$v.form.ingredientName.alpha"
                >
                  ingredient name should include letters only
                </b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.unit.alpha">
                  unit should contain letters only
                </b-form-invalid-feedback>
                <b-form-invalid-feedback v-else-if="!$v.form.unit.length">
                  unit should be up to 45 characters long
                </b-form-invalid-feedback>
              </b-form>
            </div>

            <!-- instructions -->
            <div class="instructions">
              <h5 class="input-title">add the instructions</h5>
              <b-form inline @submit.prevent="addInstructions">
                <label class="sr-only" for="inline-form-input-name"
                  >instruction</label
                >
                <b-form-textarea
                  placeholder="Instruction Step"
                  id="step-text"
                  value="instruction"
                  v-model="form.step"
                  class="mb-2 mr-sm-2 mb-sm-0"
                ></b-form-textarea>

                <b-button type="submit" variant="primary">Add</b-button>
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
import {
  required,
  maxLength,
  minValue,
  alpha,
  sameAs,
  email,
  url,
  numeric,
} from "vuelidate/lib/validators";
export default {
  name: "NewRecipe",
  created() {
    this.ingridents = [];
    this.instructionsArray = [];
    this.showIngridentsMeesage = false;
    this.showInstructionMeesage = false;
    this.IngridentsMeesage = "";
  },
  destroyed() {
    this.onReset();
  },
  data() {
    return {
      form: {
        title: "",
        image: "",
        readyInMinutes: null,
        servingSize: null,

        vegan: null,
        vegetarian: null,
        glutenFree: null,
        checked: [],
        ingredientName: "",
        amount: null,
        unit: "",
        step: "",
      },

      show: true,
    };
  },
  validations: {
    form: {
      title: {
        required,
        length: (t) => maxLength(100)(t),
        alpha,
      },
      image: {
        url,
      },
      readyInMinutes: {
        minValue: 1,
        numeric,
      },
      servingSize: {
        minValue: 1,
        numeric,
      },
      ingredientName: {
        length: (t) => maxLength(100)(t),
        alpha,
      },
      amount: {
        minValue: 0,
      },
      unit: {
        alpha,
        length: (t) => maxLength(100)(45),
      },
      step: {
        length: (t) => maxLength(1000)(t),
      },
    },
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    onSubmit(event) {
      event.preventDefault();
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.AddRecipe();
    },
    async AddRecipe() {
      let vegan = false;
      let vegetarian = false;
      let glutenFree = false;

      if (this.form.nutritious != undefined) {
        this.form.nutritious.forEach((item) => {
          if (item === "vegan") {
            vegan = 1;
          }
          if (item === "vegetarian") {
            vegetarian = 1;
          }
          if (item === "gluten-free") {
            glutenFree = 1;
          }
        });
      }
      const DOMAIN_PATH = "http://localhost:3000";
      try {
        const response = await this.axios;
        await this.axios.create({ withCredentials: true }).post(
          DOMAIN_PATH + "/users/add",
          {
            title: this.form.title,
            image: this.form.image,
            readyInMinutes: this.form.readyInMinutes,
            vegan: vegan,
            vegetarian: vegetarian,
            glutenFree: glutenFree,
            ingredientsAndQuantities: this.form.ingridents,
            instructions: this.instructionsArray,
            servingSize: this.form.servingSize,
          },
          { withCredentials: true }
        );
        this.$root.toast(
          "Recipe Added",
          "new recipe added successfully",
          "success"
        );
        this.onReset();
        console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onReset() {
      this.form = {
        title: "",
        image: "",
        readyInMinutes: null,
        servingSize: null,
        vegan: null,
        vegetarian: null,
        glutenFree: null,
        checked: [],
        ingredientName: "",
        amount: null,
        unit: "",
        step: "",
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
    addIngredient() {
      console.log("you sumbitted");

      this.showIngridentsMeesage = true;
      this.IngridentsMeesage = this.form.ingredientName;

      console.log(this.ingridents);
      this.ingridents.push({
        ingredientName: this.form.ingredientName,
        amount: this.form.amount,
        unit: this.form.unit,
      });
      console.log(this.ingridents);
      (this.form.ingredientName = ""),
        (this.form.amount = null),
        (this.form.unit = "");
    },

    addInstructions() {
      this.showInstructionMeesage = true;

      this.instructionsArray.push({
        number: this.instructionsArray.length + 1,
        step: this.form.step,
      });
      console.log(this.instructionsArray);
      console.log(this.instructionsArray.length);
      this.form.step = "";
    },
  },
};
</script>

<style scope>
.ingredients,
.instructions {
  margin-top: 18px;
}
</style>

<style>
#modal-1 .modal-dialog {
  max-width: 800px;
}

.modal-body {
  margin: auto;
}

.form-inline {
  justify-content: space-around;
}

.d-block,
.input-title,
.bv-no-focus-ring.col-form-label.pt-0,
.mr-sm-2 {
  font-size: 18px;
  font-weight: normal;
}
/* 
.mb-2.mr-sm-2.mb-sm-0.form-control {
  padding: 0px;
} */

#inline-form-input-readyTime {
  padding-top: 0px;
  padding-bottom: 0px;
}

#input-group-4 {
  margin-top: 10px;
}

.custom-control-label {
  font-size: 16px;
}

.form-control {
  width: 98.6%;
}

label {
  margin-top: 10px;
  margin-bottom: 10px;
}

.invalid-feedback {
  margin-left: 10px;
}

#step-text {
  width: 638px;
}
</style>
