<template>
    <div>
        <b-progress :max="max" height="2rem">
        <b-progress-bar :value="value" >
            <span>Progress: <strong>{{ value.toFixed(2) }} / {{ max }}</strong></span>
        </b-progress-bar>
        </b-progress>
    </div>
</template>

<script>
export default {
  name: "ProgressBar",
  props: {
    r_id:{
      type: Number,
      required: true,
    },
  },
  data() {
    return {
        max: 0,
        value: 0
    }
  },
  created() {
    this.max=0;
    this.value = getSessionValue();
    console.log("created this.value : "+this.value);
    console.log(this.r_id);
  },
  mounted() {
    if(this.getSessionValue()){
      minStep = this.getSessionValue();
      this.value = minStep;
    }

  },
  methods: {
     getSessionValue(){
      return JSON.parse(sessionStorage.making_progress)[this.r_id];
    },
  },
}
</script>
