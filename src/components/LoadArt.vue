<template>
    <div>
        <input type="file" accept="image/*" @change="handleImageChange">
        <input type="file" accept="image/*" @change="handleFrameChange">
        <canvas ref="canvas"></canvas>
        <button @click="exportImage">Export Image</button>
    </div>
</template>