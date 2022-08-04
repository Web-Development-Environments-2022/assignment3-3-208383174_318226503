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
        <!-- <b-form-group v-slot="{ ariaDescribedby }">
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
        </b-form-group> -->

        <ul class="list-group">
          <li v-for="s in steps_todo" :key="s.number" class="list-group-item">
            <div class="row">
              <div class="col">
                <span v-if="s.isDone">
                  <del>{{ s.text }}</del>
                </span>
                <span v-else>{{ s.text }}</span>
              </div>
              <div class="col-auto">
                <div class="row">
                  <div class="col" v-if="canClickButton(s)">
                    <b-button @click="done(s)" variant="outline-info">Done</b-button>
                  </div>
                  <div class="col" v-if="!canClickButton(s)">
                    <button @click="done(s)" class="btn btn-light" disabled="true">Done</button>
                  </div>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
  </div>
</template>

<script>
import { kMaxLength } from 'buffer';

export default {
  name: "Instructions",
  props: {
    instructions: {
      type: Array,
      required: true,
    },
    r_id:{
      type: Number,
      required: true,
    }
  },
  created() {
    this.max=0;
    this.value = getSessionValue();
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
    let minStep =0;
    let isDone_ = false;
    if(this.getSessionValue()){
      minStep = this.getSessionValue();
      console.log("minStep : "+minStep);
    }
    for (let i = 0; i < this.instructions.length; i++) {
        str_step = `Step ${this.instructions[i].number}: ${this.instructions[i].step}`;
        isDone_ = this.instructions[i].number<=minStep;
        if(isDone_){
          this.selected.push(parseInt(this.instructions[i].number));
        }
        this.steps_todo.push({text:str_step, value: this.instructions[i].number,isDone:isDone_});
        } 
    console.log("this.steps_todo.length : "+parseInt(this.steps_todo.length));
    // console.log("max: "+max);
    this.max = parseInt(this.steps_todo.length);
    console.log("max: "+max);
  },
  methods: {
    updateValueProgress(){
      if (this.value<this.max){
        this.value +=1;
      }
    },

    getSessionValue(){
      console.log("sessionStorage.making_progress[this.r_id] "+  JSON.parse(sessionStorage.making_progress)[this.r_id]);
      return JSON.parse(sessionStorage.making_progress)[this.r_id];
    },
    done(step){
      step.isDone = true;
      this.selected.push(parseInt(step.value));
      this.updateValueProgress();
      //save step

      let temp1 = sessionStorage.making_progress;
      console.log("temp1: "+ temp1);
      let temp = JSON.parse(temp1);
      console.log("temp: "+ temp);
      if ( temp == undefined){
        temp = {};
      }

      temp[this.r_id] = this.value;
      sessionStorage.setItem("making_progress", JSON.stringify(temp));

    },
    canClickButton(step){
      let len = this.selected.length;
      if(step.value==1 && len==0 ){
        return true;
      }
      if(parseInt(step.value)-this.selected[len-1]==1){
        return true;
      }
      return false;

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
