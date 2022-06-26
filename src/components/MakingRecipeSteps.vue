<template>
  <div>
    <h1>
      Instructions
    </h1>
    <b-progress :max="max" height="2rem">
      <b-progress-bar :value="value" >
        <span>Progress: <strong>{{ value.toFixed(2) }} / {{ max }}</strong></span>
      </b-progress-bar>
    </b-progress>
    <div class="wrapped">
        <b-form-group v-slot="{ ariaDescribedby }">
            <b-form-checkbox-group
                v-model="selected"
                :options="steps_todo"
                :aria-describedby="ariaDescribedby"
                size="lg"
                name="steps_todo_"
                stacked
            >
            <b-form-invalid-feedback :state="state">Please stick to the order of the steps</b-form-invalid-feedback>
            </b-form-checkbox-group>
        </b-form-group>
      </div>
  </div>
</template>

<script>
export default {
  name: "Instructions",
  props: {
    instructions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
        steps_todo: [],
        selected: [],
        max: 0,
        value_progress:0,
        value: 0,
    };
  },
  mounted() {
    console.log("mounted make recipe");
    let str_step ="";
    for (let i = 0; i < this.instructions.length; i++) {
        str_step = `Step ${this.instructions[i].number}: ${this.instructions[i].step}`;
        this.steps_todo.push({text:str_step, value: this.instructions[i].number});
        } 
    this.max = this.steps_todo.length;
    console.log("max: "+max);
  },
  methods: {
    updateValueProgress(){
      if (this.value<this.max){
        this.value +=1;
      }
    },
    getValue(){
        return this.value_progress;
    }
  },
    computed: {
      state() {
        console.log("this.selected:"+this.selected);
        console.log("value: "+this.value);
        console.log("max: "+this.max);
        let len = this.selected.length;
        if(len==1){
            this.updateValueProgress();
            return true;
        }
        if(len==0){
          return true;
        }
        console.log(parseInt(this.selected[len-1]));
        if(parseInt(this.selected[len-1])-parseInt(this.selected[len-2])==1){
            this.updateValueProgress();
            return true;
        }
        return false;
},
}};
</script>

<style scoped>
.step {
  font-size: 17px;
}

.specific-step {
  margin-bottom: 30px;
}

h1 {
  font-size: 23px;
  margin-bottom: 25px;
  font-family: FreeMono, monospace;
}

#step-title {
  font-family: FreeMono, monospace;
  text-decoration: underline;
}

#step-description {
  margin-top: 5px;
}

#sample-list-flat {
    border: 1px solid #dddddd;
    border-radius: 3px;
    margin: auto;
}
#flat-list {
    width: 50%;
    padding: 10px;
    margin: auto;
}
</style>
