<template>
    <div>
        <input type="file" accept="image/*" @change="handleImageChange">
        <input type="file" accept="image/*" @change="handleFrameChange">
        <canvas ref="canvas"></canvas>
        <button @click="exportImage">Export Image</button>
    </div>
</template>

<script>
import { read } from 'fs';

export default{
    Data() {
        return {
            image: null,
            frame: null,
            canvas:null,
        };
    },
    mounted() {
        this.canvas = this.$refs.canvas;
    },
    methods: {
        handleImageChange(e) {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = () => {
                const img = new Image();
                img.src = reader.result;
                img.onload = () => {
                    this.image = img;
                };
            };
            reader.readAsDataURL(file);
        },
        handleFrameChange(e) {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = () => {
                const img = new Image();
                img.src = reader.result;
                img.onload = () => {
                    this.frame = img;
                };
            };
            reader.readAsDataURL(file);
        },
        

    }
}
    
</script>