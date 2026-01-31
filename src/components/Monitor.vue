<template>
  <div :style="`width: ${maxWidth}px; min-width: 180px; text-align: left;`">
    <div class="monitor" :style="style">
      <Window :ppi="ppi" :windows-scale-factor="nscalefactor"/>
    </div>
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
        <td>Scaling</td>
        <td>
          <div class="input-container" style="width: 60px">
            <select
              style="width: 100%; border: none"
              v-model="nscalefactor"
            >
              <option value="1">100%</option>
              <option value="1.25">125%</option>
              <option value="1.50">150%</option>
              <option value="1.5">175%</option>
              <option value="2">200%</option>
            </select>
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
      <tr>
        <td>Scaled PPI:</td>
        <td>
          <div class="input-container" style="color: #666">
            {{ (ppi / nscalefactor).toFixed(1) }}
          </div>
        </td>
      </tr>
    </table>
    <div style="text-align: center">
      <button title="move monitor left" @click="$emit('move-left')"><i class="fa fa-arrow-left" ></i></button>
      <button title="remove monitor" @click="$emit('remove')" ><i class="fa fa-trash-can"></i></button>
      <button title="rotate monitor" @click="rotate()"><i class="fa fa-rotate-left"></i></button>
      <button title="move monitor right" @click="$emit('move-right')"><i class="fa fa-arrow-right"></i></button>
    </div>
  </div>
</template>

<style>
.monitor {
  background-color: #bbb;
  border: 2px solid black;
  margin: 10px;
  display: inline-block;
  overflow: hidden;
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
select,
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
import Window from "./Window.vue"

export default {
  name: "Monitor",
  components: {
    Window,
  },
  props: ["width", "height", "diagonal", "scalingFactor"],
  data: function () {
    return {
      nwidth: this.width,
      nheight: this.height,
      ndiagonal: this.diagonal,
      nscalefactor: 1
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
    rotate() {
      const origWidth = this.nwidth;
      this.nwidth = this.nheight;
      this.nheight = origWidth;
    }
  },
};
</script>
