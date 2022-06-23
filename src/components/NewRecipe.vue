<template>
  <div>
    <b-modal id="modal-1" title="Create Your New Recipe">
     <div>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">

      <!-- title -->
      <b-form-group
        id="title"
        label="Title:"
        label-for="input-1"
      >
        <b-form-input
          id="input-1"
          v-model="form.title"
          type="text"
          placeholder="Enter your recipe title"
          required
        ></b-form-input>
      </b-form-group>

      <!-- image -->
      <b-form-group id="input-group-2" label="Image url:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.image"
          placeholder="Enter your recipe's image url"
          required
        ></b-form-input>
      </b-form-group>

      <!-- readyInMinutes -->
      <b-form-group id="input-group-3" label="Making time:" label-for="input-3">
        <b-form-input
          id="input-3"
          type="number"
          v-model="form.readyInMinutes"
          placeholder="How many minutes does it takes to make the recipe"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group label="select if the following maches your recipe" id="input-group-4" v-slot="{ ariaDescribedby } ">
        <b-form-checkbox-group
          v-model="form.checked"
          id="checkboxes-4"
          :aria-describedby="ariaDescribedby"
        >
          <b-form-checkbox value="vegan">Vegan</b-form-checkbox>
          <b-form-checkbox value="vegeterian">Vegeterian</b-form-checkbox>
          <b-form-checkbox value="gluten-free">Gluten Free</b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card>
  </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "NewRecipe",
 data() {
      return {
        form: {
          title: '',
          image: '',
          readyInMinutes: null,
          vegan: null,
          vegetarian: null,
          glutenFree: null,
          checked: []
        },
        foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
        show: true
      }
    },
    methods: {
      onSubmit(event) {
        event.preventDefault()
        alert(JSON.stringify(this.form))
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
};
</script>

<style scoped>
