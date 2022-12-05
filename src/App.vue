<template>
  <div id="app">
    <button @click="generateReport">GENERATE</button>
    <div class="generate-pdf" ref="generatePDF">
      <div class="image">
        <img src="@/assets/photo.jpg" alt="Model" />
      </div>
      <div class="content">
        <h1>Model Image - Browse</h1>
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
  methods: {
    async generateReport() {
      const box = this.$refs.generatePDF;
      let doc = new jsPDF({
        orientation: "portrait",
        unit: "px",
        format: [984, 792],
      });

      await html2canvas(box, {
        orientation: "portrait",
        unit: "in",
        format: [8.25, 10.25],
      }).then((canvas) => {
        doc.addImage(canvas.toDataURL("image/png"), "PNG", 12, 14, 768, 960);
      });

      doc.save("File.pdf");
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
</style>
