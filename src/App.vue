<template>
  <div id="app">
    <div class="main">
      <canvas class="canvas" ref="canvas" width="1280" height="720"></canvas>
    </div>
    <input v-model="text" placeholder="Enter your text here" />
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      text: 'ABCDE',
      image: null,
    };
  },
  watch: {
    text() {
      this.drawText();
    },
  },
  mounted() {
    this.loadImage();
  },
  methods: {
    loadImage() {
      const img = new Image();
      img.src = '/images/base.png';  // 画像のパスを指定
      img.onload = () => {
        this.image = img;
        this.drawText();  // 画像を読み込んだ後にテキストを描画
      };
    },
    drawText() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);  // キャンバスをクリア
      if (this.image) {
        ctx.drawImage(this.image, 0, 0, canvas.width, canvas.height);  // 画像を描画
      }
      ctx.font = '30px Arial';
      ctx.fillStyle = 'black';
      ctx.fillText(this.text, 10, canvas.height - 10);  // テキストを描画
    },
    saveImage() {
      html2canvas(this.$refs.canvas).then((canvas) => {
        const link = document.createElement('a');
        link.href = canvas.toDataURL('image/png');
        link.download = 'meme.png';
        link.click();
      });
    },
  },
};
</script>

<style>
</style>
