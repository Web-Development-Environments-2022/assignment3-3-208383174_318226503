<template>
    <div>
        <b-progress :max="max" height="2rem" class="mb-3" >
        <b-progress-bar :value="value" >
            <span><strong>{{ value.toFixed(2) }} / {{ max }}</strong></span>
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

  },
  mounted() {
    this.max=this.get_Session_Value()[1];
    this.value = this.get_Session_Value()[0];
    console.log("recipe progress bar this.value : "+this.value);
    console.log(this.r_id);
    console.log("recipe progress bar this.max : "+this.max);

  },
  methods: {
     get_Session_Value(){
        console.log(JSON.parse(sessionStorage.making_progress)[this.r_id]);
      return JSON.parse(sessionStorage.making_progress)[this.r_id];
    },
  },
}
</script>
