<template>
    <div>
        <label>
            Choose file:
        </label>
        <input type="file" @change="loadFile($event)">
        <p>
          {{currentText}}
        </p>
        <button id="download-pdf" :disabled="dlBtnDis" @click="downloadPdf()">Download PDF</button>
    </div>
</template>

<script>

import Tesseract from 'tesseract.js';
import { jsPDF } from "jspdf";

export default {
    name: 'LoadFile',
    data() {
        return {
            currentText: 'No text',
            dlBtnDis: true
        }
    },
    methods: {
        loadFile: function(event) {
            this.eventObj = event.target.files[0];
            this.processFile(event).then((text) => {
                console.log(text);
                this.currentText = text;
                this.dlBtnDis = false;
            });
        },
        processFile: async (event) => {
            console.log(event);
            console.log(event.target.files[0]);

            const image = event.target.files[0];

            console.log(`Recognizing ${image}`);
            let output ;
            await Tesseract.recognize(
                event.target.files[0],
                'eng',
                { logger: m => console.log(m) }
            ).then(({ data: { text } }) => {
                console.log(text);
                output = text;
            });
            return output;
        },
        downloadPdf() {
            const doc = new jsPDF();

            doc.text(this.currentText, 10, 10);
            doc.save("test.pdf");
        }
    }
}

</script>

<style scoped>
</style>