<template>
    <div>
        <button v-on:click="loadFile">recognize</button>
        <img id="text-img" alt="Vue logo" src="./../assets/testocr.png">
        <p>
          {{currentText}}
        </p>
    </div>
</template>

<script>

import { PSM, OEM } from 'tesseract.js';

const { createWorker } = require("tesseract.js");
const worker = createWorker({
  logger: m => console.log(m),
});

export default {
  name: 'ExampleTesseract',
  data() {
    return {
      currentText: 'No text',
    }
  },
  methods: {
    loadFile: function() {
      this.recognize().then((text) => {
            console.log(text);
            this.currentText = text;
        });
    },
    recognize: async () => {
      const img = document.getElementById('text-img');
      console.log(img);
      await worker.load();
      await worker.loadLanguage('eng');
      await worker.initialize('eng', OEM.LSTM_ONLY);
      await worker.setParameters({
        tessedit_pageseg_mode: PSM.SINGLE_BLOCK,
      });
      const { data: { text } } = await worker.recognize(img);
      console.log(text);
      return text;
    }
  }
}

</script>

<style scoped>
</style>