<template>
  <div :style="`width: ${maxWidth}px; min-width: 180px; text-align: left;`">
    <div class="monitor" :style="style"></div>
    <table>
      <tr>
        <td>Diagonal:</td>
        <td>
          <div class="input-container" style="width: 25px">
            <input
              onClick="this.select();"
              style="text-align: right; width: calc(100% - 2px)"
              type="number"
              v-model="ndiagonal"
            />"
          </div>
        </td>
      </tr>
      <tr>
        <td>Resolution:</td>
        <td>
          <div class="input-container" style="width: 80px">
            <input
              onClick="this.select();"
              style="text-align: right; width: calc(50% - 5px)"
              type="numuber"
              v-model="nwidth"
            />
            &times;
            <input
              onClick="this.select();"
              style="text-align: left; width: calc(50% - 10px)"
              type="number"
              v-model="nheight"
            />
          </div>
        </td>
      </tr>
      <tr>
        <td>Aspect ratio:</td>
        <td>
          <div class="input-container" style="color: #666">
            {{ aspectRatio }}
          </div>
        </td>
      </tr>
      <tr>
        <td>Dimensions:</td>
        <td>
          <div class="input-container" style="color: #666">
            {{ cmWidth.toFixed(1) }}cm &times; {{ cmHeight.toFixed(1) }}cm
          </div>
        </td>
      </tr>
      <tr>
        <td>PPI:</td>
        <td>
          <div class="input-container" style="color: #666">
            {{ ppi.toFixed(1) }}
          </div>
        </td>
      </tr>
    </table>
    <div style="text-align: center">
      <button @click="$emit('remove')" style="display: inline">remove</button>
    </div>
  </div>
</template>

<style>
.monitor {
  background-color: #bbb;
  border: 2px solid black;
  margin: 10px;
  display: inline-block;
}
.input-container {
  text-align: left;
  display: inline-block;
  background-color: #eee;
  padding: 5px 10px 5px 5px;
  margin: 2px 0px;
  border: 0px;
  border-radius: 5px;
}
input,
input:active,
input:focus {
  font-family: Arial, sans-serif;
  font-size: 13px;
  border: 0px solid black;
  outline: none;
  margin: 0;
  padding: 0;
  background-color: #eee;
  -moz-appearance: textfield;
  appearance: textfield;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>

<script>
export default {
  name: "Monitor",
  props: ["width", "height", "diagonal", "scalingFactor"],
  data: function () {
    return {
      nwidth: this.width,
      nheight: this.height,
      ndiagonal: this.diagonal,
    };
  },
  computed: {
    style() {
      return `
            width: ${this.cmWidth * this.scalingFactor}px;
            height: ${this.cmHeight * this.scalingFactor}px;
            margin: 10px
        `;
    },
    maxWidth() {
      return Number(this.cmWidth) * this.scalingFactor + 20;
    },
    aspectRatio() {
      let commonAr = {
        "8:5": "16:10",
      };
      let gcd = this.gcd(this.nwidth, this.nheight);
      let ar = `${this.nwidth / gcd}:${this.nheight / gcd}`;
      if (ar in Object.keys(commonAr)) {
        ar = commonAr[ar];
      }
      return ar;
    },
    cmHeight() {
      let ar = this.nwidth / this.nheight;
      return Math.sqrt(this.ndiagonal ** 2 / (1 + ar ** 2)) * 2.54;
    },
    cmWidth() {
      let ar = this.nwidth / this.nheight;
      return Math.sqrt(this.ndiagonal ** 2 / (1 + 1 / ar ** 2)) * 2.54;
    },
    ppi() {
      return (Number(this.nheight) / this.cmHeight) * 2.54;
    },
  },
  methods: {
    gcd(a, b) {
      if (!b) {
        return a;
      }
      return this.gcd(b, a % b);
    },
  },
};
</script>
