<template>
  <div class="container">
    <h1 v-if="color" :style="{ color: textColor }">
      The generated color is 
      <span class="textColor" @click="changeTypeColor" :style="{ color: textColor }">
        {{ color[type] }}
      </span>
    </h1>

    <br /><div class="buttonContainer">
      <button
        class="generateButton"
        @click="setBackgroundColor(randomColor())"
        :style="{ color: textColor }"
      >Generate Random Color</button>

      <button class="copyButton" v-if="color" @click="copyColor" :style="{ color: textColor }">
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
      rgb: null,
      textColor: '#fff',
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
      
      this.rgb = { R, G, B };
      return { RGB: `rgb(${R}, ${G}, ${B})`, HEX };
    },
    setBackgroundColor(color) {
      const textColor = (
        this.rgb["R"] * 299
        + this.rgb["G"] * 587
        + this.rgb["B"] * 114
      ) / 1000 < 128 ? '#fff' : 'rgba(0,0,0,.87)';

      this.color = color;
      this.textColor = textColor;
      document.body.style.backgroundColor = color[this.type];
    },
    copyColor({ target }) {
      const selectedColor = this.color[this.type]
      navigator.clipboard.writeText(selectedColor);
      target.style.color = selectedColor;
      target.style.transition = 'all 1s';
      setTimeout(() => (target.style.color = '#AFE1AF'), 500);
    },
    changeTypeColor() {
      this.type = this.type == 'HEX' ? 'RGB' : 'HEX'
    }
  },
  created () {
    this.setBackgroundColor(
      this.randomColor()
    );
  }
}
</script>