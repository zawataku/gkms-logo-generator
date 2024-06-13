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
      text: '',
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
      ctx.font = '145px MainFont';
      ctx.fillStyle = 'black';
      ctx.textAlign = 'center';  // テキストを中央揃えに設定
      const x = canvas.width / 2;  // キャンバスの幅の中央
      const y = 450;  // 上から100pxの位置
      ctx.fillText(this.text, x, y);  // テキストを描画
    },
  },
};
</script>
