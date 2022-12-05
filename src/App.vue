<template>
  <div id="app">
    <button @click="generateReport">GENERATE PDF</button>
    <div v-if="loading1">Generating Page 1 ...</div>
    <div v-if="loading2">Generating Page 2 ...</div>
    <hr style="margin-top: 0;">
    <!-- Page 1 Image -->
    <div class="generate-pdf" ref="generatePDF">
      <div class="image">
        <img src="@/assets/photo.jpg" alt="Model" />
      </div>
      <div class="content">
        <h1>Model Image - Browse</h1>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
      </div>
    </div>
    <hr>
    <!-- Page 2 Image -->
    <div class="generate-pdf" ref="generatePDF1">
      <div class="image">
        <img src="@/assets/photo1.jpg" alt="Model" />
      </div>
      <div class="content">
        <h1>Model Image 1 - Browse</h1>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
      </div>
    </div>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";
import html2canvas from "html2canvas";
export default {
  name: "App",
  data(){
    return {
      loading1: false,
      loading2: false,
    }
  },
  methods: {
    async generateReport() {
      const image1 = this.$refs.generatePDF;
      const image2 = this.$refs.generatePDF1;

      /* Basic Setup of PDF */
      let doc = new jsPDF({
        orientation: "portrait",
        unit: "px",
        format: [984, 792],
      });

      /* First Page */
      this.loading1 = true;
      await html2canvas(image1, {
        orientation: "portrait",
        unit: "in",
        format: [8.25, 10.25],
      }).then((canvas) => {
        doc.addImage(canvas.toDataURL("image/png"), "PNG", 12, 14, 768, 960);
        this.loading1 = false;
        this.loading2 = true;
      });

      /* Second Page */
      doc.addPage();
      await html2canvas(image2, {
        orientation: "portrait",
        unit: "in",
        format: [8.25, 10.25],
      }).then((canvas) => {
        doc.addImage(canvas.toDataURL("image/png"), "PNG", 12, 14, 768, 960);
        this.loading2 = false;
      });

      /* Save as PDF */
      doc.save("Image.pdf");
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}
body{
  background: #f0f0f0;
}
#app{
  max-width: 808px;
  margin: 20px auto;
  background: #FFFFFF;
  padding: 20px;
  border-radius: 20px;
}
.generate-pdf {
  position: relative;
  width: 768px;
  height: 960px;
}
.image {
  width: 100%;
  height: 100%;
}
.image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.content {
  width: 100%;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 20px;
  text-align: center;
  position: absolute;
  left: 0;
  bottom: 30px;
}
hr{
  margin: 20px 0;
}

button{
  margin: 10px auto;
  padding: 5px 10px;
}
</style>
