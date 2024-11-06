<template>
  <div class="controls" v-if="showControls">
    <div class="btn-solve" @click="solvePuzzle">Solve</div>
    <div class="btn-mix" @click="mixPuzzle">Mix</div>

    <select v-model="puzzleViewMode" @change="selectPuzzleViewMode">
      <option value="All">All</option>
      <option value="BorderPieces">Only Border Pieces</option>
      <option value="NonBorderPieces">Only Non Border Pieces</option>
    </select>

    <div class="divider"></div>
    <span>Show Hints: </span>
    <input type="checkbox" v-model="puzzleHintsEnabled" @change="toggleHints">
  </div>

  <div class="jigsaw-puzzle">
    <canvas ref="canvas" :width="canvasWidth" :height="canvasHeight"></canvas>
  </div>
</template>

<script>
import Puzzle from "@/assets/lib/puzzle.js";

export default {
  props: {
    canvasWidth: {
      type: Number,
      default: 1512,
    },
    canvasHeight: {
      type: Number,
      default: 462,
    },
    showControls: {
      type: Boolean,
      default: false,
    },
    imageUrl: {
      type: String,
      required: true,
    },
    rows: {
      type: Number,
      default: 15,
    },
    columns: {
      type: Number,
      default: 20,
    },
    maxImageWidth: {
      type: Number,
      default: 60,
    },
    maxImageHeight: {
      type: Number,
      default: 75,
    },
    solveRandom: {
      type: Boolean,
      default: false,
    },
    hintsEnabled: {
      type: Boolean,
      default: false,
    },
    scaleMultiplier: {
      type: Number,
      default: 1,
    },
    animationDuration: {
      type: Number,
      default: 250,
    },
  },
  data() {
    return {
      puzzle: null,
      puzzleViewMode: 'All',
      puzzleHintsEnabled: false,
    };
  },
  mounted() {
    this.puzzleHintsEnabled = this.hintsEnabled;
    this.initPuzzle();
  },
  methods: {
    initPuzzle() {
      const canvas = this.$refs.canvas;
      const img = new Image();
      img.src = this.imageUrl;

      img.onload = () => {
        this.puzzle = new Puzzle({
          canvas: canvas,
          image: img,
          rows: this.rows,
          columns: this.columns,
          maxImageWidth: this.maxImageWidth,
          maxImageHeight: this.maxImageHeight,
          solveRandom: this.solveRandom,
          hintsEnabled: this.hintsEnabled,
          scaleMultiplier: this.scaleMultiplier,
          animationDuration: this.animationDuration,
        });

        this.puzzle.generatePieces();
        this.puzzle.randomizePieces();
        requestAnimationFrame(this.puzzle.draw.bind(this.puzzle));
      };
    },

    solvePuzzle() {
      this.puzzle.solve();
    },
    mixPuzzle() {
      this.puzzle.randomizePieces();
    },
    selectPuzzleViewMode() {
      this.puzzle.viewMode = this.puzzleViewMode;
    },
    toggleHints() {
      this.puzzle.toggleHints(this.puzzleHintsEnabled);
    }
  },
};
</script>

<style scoped>
.jigsaw-puzzle {
  display: flex;
  justify-content: center;
  align-items: center;
}

canvas {
  z-index: 5;
  width: 100% !important;
  height: 100% !important;
}

.controls {
  width: 100%;
  height: 50px;
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  z-index: 20;
  background-color: #353535;
  box-shadow: 0 0 16px 2px rgba(0, 0, 0, .15);
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding: 0 10px;
}

.controls .btn-solve,
.controls .btn-mix {
  width: 75px;
  height: 30px;
  margin-right: 10px;
  font-family: 'Poppins', sans-serif;
  background-color: green;
  color: white;
  font-weight: bold;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0;
}

.controls select {
  width: 200px;
  height: 30px;
  margin-right: 10px;
  font-family: 'Poppins', sans-serif;
  background-color: #000;
  color: white;
  font-weight: bold;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0;
  border: none;
  outline: none;
}

.controls .divider {
  margin-right: 10px;
  width: 2px;
  height: calc(100% - 20px);
  background-color: #707070;
}

.controls span {
  color: #fff;
  font-size: 16px;
  font-weight: normal;
  margin-right: 10px;
}
</style>
