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

/*
const { createWorker } = Tesseract;
const worker = createWorker({
    corePath: '/node_modules/tesseract.js-core/tesseract-core.wasm.js',
    logger: m => console.log(m),
});
*/

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
            this.processFile(event).then((text) => {
                console.log(text);
                this.currentText = text;
                this.dlBtnDis = false;
            });
        },
        downloadPdf: function() {
            this.downloadPDF(this.currentText).then(() => {
                console.log("teste");
            })
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
        downloadPDF: async (currentText) => {
            const filename = 'tesseract-ocr-result.pdf';
            const data = currentText;
            //const { data } = await worker.getPDF('Tesseract OCR Result');
            const blob = new Blob([new Uint8Array(data)], { type: 'application/pdf' });
            if (navigator.msSaveBlob) {
                navigator.msSaveBlob(blob, filename);
            } else {
                const link = document.createElement('a');
                if (link.download !== undefined) {
                    const url = URL.createObjectURL(blob);
                    link.setAttribute('href', url);
                    link.setAttribute('download', filename);
                    link.style.visibility = 'hidden';
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                }
            }
        }
    }
}

</script>

<style scoped>
</style>