<template>
  <div id="app">
    <JigsawPuzzle :imageUrl="puzzleImage" :rows="15" :columns="20" :showControls="true" />
  </div>
</template>

<script>
import JigsawPuzzle from "./components/JigsawPuzzle.vue";

export default {
  name: "App",
  components: {
    JigsawPuzzle,
  },
  data() {
    return {
      puzzleImage: require('@/assets/image1.png')
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  user-select: none !important;
  font-family: 'Poppins', sans-serif;
}

body,
html {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  background-color: #454545;
  overflow: auto;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
