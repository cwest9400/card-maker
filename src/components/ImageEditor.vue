<template>
    <div>
      <input type="file" @change="onFileChange" />
      <canvas ref="canvas" :width="canvasWidth" :height="canvasHeight"></canvas>
      <div class="frames">
        <div v-for="(frame, index) in frames" :key="index" :style="{backgroundImage: 'url(' + frame.url + ')'}" @click="selectFrame(index)" :class="{selected: index === selectedFrame}">
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        canvasWidth: 500,
        canvasHeight: 500,
        frames: [
          {
            url: 'https://i.imgur.com/2SFGpQ2.jpeg',
            width: 400,
            height: 400,
            x: 78,
            y: 77,
          },
          {
            url: 'https://i.imgur.com/eiQ2oXT.png',
            width: 324,
            height: 324,
            x: 88,
            y: 88,
          },
          {
            url: 'https://i.imgur.com/M1bSo7V.png',
            width: 450,
            height: 450,
            x: 25,
            y: 25,
          },
        ],
        selectedFrame: 0,
        image: null,
      };
    },
    methods: {
      onFileChange(e) {
        const file = e.target.files[0];
        const reader = new FileReader();
  
        reader.onload = (event) => {
          const img = new Image();
          img.onload = () => {
            this.image = img;
            this.drawCanvas();
          };
          img.src = event.target.result;
        };
  
        reader.readAsDataURL(file);
      },
      selectFrame(index) {
        this.selectedFrame = index;
        this.drawCanvas();
      },
      drawCanvas() {
        const canvas = this.$refs.canvas;
        const context = canvas.getContext('2d');
  
        // Clear canvas
        context.clearRect(0, 0, canvas.width, canvas.height);
  
        // Draw frame
        const frame = this.frames[this.selectedFrame];
        const frameImg = new Image();
        frameImg.src = frame.url;
        frameImg.onload = () => {
          context.drawImage(frameImg, 0, 0, canvas.width, canvas.height);
        };
  
        // Draw image
        if (this.image) {
          context.globalCompositeOperation = 'source-atop';
          context.drawImage(this.image, frame.x, frame.y, frame.width, frame.height);
        }
      },
    },
  };
  </script>
  
  <style>
  .frames {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .frames > div {
    width: 100px;
    height: 100px;
    background-size: contain;
    background-repeat: no-repeat;
    margin: 10px;
    cursor: pointer;
  }
  
  .frames > div.selected {
    border: 2px solid #007bff;
  }
  </style>
  