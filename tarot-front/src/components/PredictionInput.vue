<template>
    <div class="prediction-input">
      <label for="prediction-text">请输入您的预测问题：</label>
      <input id="prediction-text" v-model="prediction" type="text" />
      <button @click="submitPrediction">预测</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        prediction: '',
      };
    },
    methods: {
      submitPrediction() {
        this.$emit('submit-prediction', this.prediction);
        console.log('1111')
        this.prediction = '';
      },
    },
  };
  </script>
  
  <style scoped>
  .prediction-input {
    /* 在此处添加样式 */
  }
  </style>
  