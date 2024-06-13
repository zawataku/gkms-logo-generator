<template>
  <div id="app">
    <div class="inner">
      <div class="header">
          <h1 class="inner">学マスっぽいロゴジェネレータ</h1>
      </div>
      <div class="main">
        <canvas class="canvas" ref="canvas" width="1280" height="720"></canvas>
      </div>
      <input v-model="text0" placeholder="Type Here" />
      <input v-model="text1" placeholder="Type Here" />
      <input v-model="text2" placeholder="Type Here" />
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      text0: '',
      text1: '',
      text2: '',
      image: null,
    };
  },
  watch: {
    text0() {
      this.drawText();
    },
    text1() {
      this.drawText();
    },
    text2() {
      this.drawText();
    },
  },
  mounted() {
    this.loadImage();
  },
  methods: {
    loadImage() {
      const img = new Image();
      img.src = '/images/base1.png';  // 画像のパスを指定
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
      ctx.textAlign = 'center';  // テキストを中央揃えに設定
      const x = canvas.width / 2;  // キャンバスの幅の中央

      ctx.font = '22px Font_0';  // ここでフォントを変更
      ctx.fillStyle = 'white';
      const y0 = 276;
      ctx.fillText(this.text0, x, y0);  // テキストを描画

      ctx.font = '127px Font_1';
      ctx.fillStyle = 'black';
      const y1 = 453;
      ctx.fillText(this.text1, x, y1);  // テキストを描画

      ctx.font = '40px Font_0';  // ここでフォントを変更
      ctx.fillStyle = '#79C4B5';
      const y2 = 530;
      ctx.fillText(this.text2, x, y2);  // テキストを描画

    },
  },
};
</script>
