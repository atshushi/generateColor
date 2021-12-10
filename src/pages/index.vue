<template>
  <div class="container">
    <h1 v-if="color">
      The generated color is 
      <span class="textColor" @click="changeTypeColor">
        {{ color[type] }}
      </span>
    </h1>

    <br /><div class="buttonContainer">
      <button
        class="generateButton"
        @click="setBackgroundColor(randomColor())"
      >Generate Random Color</button>

      <button class="copyButton" v-if="color" @click="copyColor">
        <i class="fas fa-copy"></i> Copy
      </button>
    </div>
  </div>
</template>

<script>
import "../../public/stylesheet/index.css";
export default {
  name: "home",
  data() {
    return {
      color: null,
      type: "HEX"
    };
  },
  methods: {
    randomColor() {
      /**
       * @author 5antos#4876
       * @repository { https://github.com/5antos/JS-Randomness/blob/master/randomColor.js }
       */
      const [R, G, B] = [
        ~~(Math.random() * 255),
        ~~(Math.random() * 255),
        ~~(Math.random() * 255)
      ];
      const HEX =
        '#' + ((1 << 24) + (R << 16) + (G << 8) + B).toString(16).slice(1);
      return { RGB: `rgb(${R}, ${G}, ${B})`, HEX };
    },
    setBackgroundColor(color) {
      this.color = color;
      document.body.style.backgroundColor = color[this.type];
    },
    copyColor({ target }) {
      const selectedColor = this.color[this.type]
      navigator.clipboard.writeText(selectedColor);
      target.style.color = selectedColor;
      setTimeout(() => (target.style.color = 'rgba(255, 255, 255, 0.8)'), 500);
    },
    changeTypeColor() {
      this.type = this.type == 'HEX' ? 'RGB' : 'HEX'
    }
  }
}
</script>