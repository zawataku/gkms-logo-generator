<template>
  <div id="app">
    <div class="inner">
      <div class="header mb-4">
        <h1 class="header-inner">学マスっぽいロゴジェネレータ</h1>
      </div>
      <div class="main">
        <canvas class="canvas" ref="canvas" width="1280" height="720"></canvas>
        <img class="output-image" ref="outputImage" :src=imageDataUrl alt="Generated Image" />
      </div>
      <div class="forms">
        <p>右クリックメニュー「名前を付けて画像を保存」またはロングタップで保存できます</p>
        <input class="form-control my-2 mx-auto" v-model="text0" placeholder="Type Here" />
        <input class="form-control my-2 mx-auto" v-model="text1" placeholder="Type Here" />
        <input class="form-control my-2 mx-auto" v-model="text2" placeholder="Type Here" />
      </div>
      <div class="footer mt-5">
        <p>Version 1.0.1</p>
        <p>Licensed under the MIT License</p>
        <a href="https://github.com/zawataku/gkms-logo-generator">GitHub Repository</a>
      </div>
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      text0: 'HATSUBOSHI GAKUEN',
      text1: '学園アイドルマスター',
      text2: 'THE iDOLM@STER',
      image: null,
      fontsLoaded: false,
      imageDataUrl: ''
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
    this.loadResources();
  },
  methods: {
    async loadResources() {
      await this.loadFonts();
      await this.loadImage();
      this.drawText();
    },
    loadFonts() {
      return new Promise((resolve) => {
        const font0 = new FontFace('Font_0', 'url(/fonts/SourceHanSansJP-Bold.woff2)');
        const font1 = new FontFace('Font_1', 'url(/fonts/851tegaki_zatsu.woff2)');
        Promise.all([font0.load(), font1.load()]).then((loadedFonts) => {
          loadedFonts.forEach((font) => document.fonts.add(font));
          this.fontsLoaded = true;
          resolve();
        });
      });
    },
    loadImage() {
      return new Promise((resolve) => {
        const img = new Image();
        img.src = '/images/base1.png'; // 画像のパスを指定
        img.onload = () => {
          this.image = img;
          resolve();
        };
      });
    },
    drawText() {
      if (!this.fontsLoaded || !this.image) return; // フォントと画像がロードされるのを待つ

      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height); // キャンバスをクリア
      if (this.image) {
        ctx.drawImage(this.image, 0, 0, canvas.width, canvas.height); // 画像を描画
      }
      ctx.textAlign = 'center'; // テキストを中央揃えに設定
      const x = canvas.width / 2; // キャンバスの幅の中央

      // テキスト0を描画
      ctx.font = '22px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'white';
      const y0 = 278;
      ctx.fillText(this.text0, x, y0); // テキストを描画

      // テキスト1を描画
      ctx.font = '122px Font_1';
      ctx.fillStyle = 'black';
      const y1 = 453;
      ctx.fillText(this.text1, x, y1); // テキストを描画

      // テキスト2を描画
      ctx.font = '40px Font_0'; // ここでフォントを変更
      ctx.fillStyle = '#79C4B5';
      const y2 = 550;
      ctx.fillText(this.text2, x, y2); // テキストを描画

      this.imageDataUrl = canvas.toDataURL('image/png');
    },
  },
};
</script>
