
<style lang="scss">
@use "~/assets/scss/main.scss";
</style>
<script setup>
import VueQrcode from '@chenfengyuan/vue-qrcode';

import { ref } from "vue";
import html2canvas from "html2canvas";
import { jsPDF } from "jspdf";
const userInput = ref("");


    // Reference to the content that will be converted to PDF
    const pdfContent = ref(null);

    const generatePDF = async () => {
      try {
        // Ensure the content is captured using html2canvas
        const canvas = await html2canvas(pdfContent.value, {
          scale: 1, // Increase scale for better quality
        });

        // Convert the canvas to an image
        const imgData = canvas.toDataURL("image/jpeg");

        // Create a jsPDF instance
        const pdf = new jsPDF({
          orientation: "portrait",
          unit: "px",
        });

        // Add the image to the PDF
        pdf.addImage(imgData, "JPEG", 0, 0, canvas.width, canvas.height);

        // Save the PDF
        pdf.save("example.pdf");
      } catch (error) {
        console.error("Error generating PDF:", error);
      }
    };
  

    


</script>

<template>
  <header>
    <span>QR POC</span>
  
  </header>

  <div class="container">
    <div class="left">
      <button @click="generatePDF()">Download PDF</button>
      <textarea
        class="user-input-field"
        placeholder="Enter your text here"
        v-model="userInput"
      ></textarea>
     
    </div>

    <div ref="pdfContent" class="pdf-content right">
      <template v-if="userInput && userInput.length > 0">
        <div class="qrcode-box">
          <div
            v-if="userInput && userInput.length > 0"
            class="qrcode"
            ref="templateReference"
          ><vue-qrcode :value="userInput"  :options="{
    width: 200,
    color: { dark: '#b00', light: '#ffffff' },
  }"></vue-qrcode>
        </div>
        </div>
      </template>

      <div v-else class="placeholder">
        <p>Enter some text on the left to generate a code</p>
      </div>
    </div>
  </div>
</template>
