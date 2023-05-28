<template>
  <div id="app">
    <PredictionInput v-if="!predictionSubmitted" @submit-prediction="submitPrediction" />
    <DrawCards v-else />
  </div>
</template>
<script>
import DrawCards from './components/DrawCards.vue';
import PredictionInput from './components/PredictionInput.vue';

export default {
  components: {
    DrawCards,
    PredictionInput,
  },
  data() {
    return {
      predictionSubmitted: false,
    };
  },
  methods: {
    submitPrediction(prediction) {
      console.log('用户的预测问题:', prediction);
      console.log('22222')
      this.predictionSubmitted = true;
    },
  },
};
</script>
<style >
body{
  margin: 0;
  padding: 0;
}
#app {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
}
 .container{
  
  width:100%;
  height:100%;
  padding: 0;
  margin: 0;
 }
</style>
