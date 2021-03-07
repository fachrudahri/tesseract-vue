<template>
  <div id="app">
    <div class="box-image">
      <img id="text-img" alt="Vue logo" src="./assets/indomie.jpeg">
    <div>
      <button v-on:click="recognize">recognize</button>
    </div>
    </div>
    <div v-if="!result">
      <p>wait</p>
    </div>
    <div v-else>
      <div class="box-result">
        <p>{{result}}</p>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import { createWorker, PSM, OEM } from 'tesseract.js';
const worker = createWorker({
  logger: m => console.table(m),
});

export default {
  name: 'app',
  data() {
    return {
      result: null
    }
  },
  methods: {
    async recognize() {
      const img = document.getElementById('text-img');
      console.log(img);
      await worker.load();
      await worker.loadLanguage('eng');
      await worker.initialize('eng', OEM.LSTM_ONLY);
      await worker.setParameters({
        tessedit_pageseg_mode: PSM.SINGLE_BLOCK,
      });
      const { data: {text} } = await worker.recognize(img);
      console.log(text)
      this.result = text
    }
  }
}
</script>

<style>
img
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

p {
  text-align: center;
}

.box-image {
  width: 400px;
  margin: 20px auto;
  padding: 10px;
}

.box-image img {
  width: 100%;
}

.box-result {
  width: 300px;
  background: white;
  margin: 20px auto;
  padding: 10px;
}
</style>
