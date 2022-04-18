<template>
  <div class="main" :style="`background-color: ${generatedColor}`">
    <div class="color">
      <div class="center">
        <h1 :style="`color: ${invertedColor}`">
          <span @click="copyHex">
            {{ generatedColor }}
            <img class="copy" src="../../public/img/copy.png" alt="copy" />
            <br>
            <span v-if="copiedHex" class="copied">
              copied
            </span>
          </span>
          <br>
          <span @click="copyRgb" class="is-rgb">
            {{ rgbValue }}
            <img class="copy" src="../../public/img/copy.png" alt="copy" />
            <br>
            <span v-if="copiedRgb" class="copied">
              copied
            </span>
          </span>
        </h1>
      </div>
      <br>
      <div class="bottom-conter">
        <button @click="generateColor" class="button is-secondary is-color-button">
          New Color
          <i class="fas fa-brush" />
        </button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  components: {
  },
  data () {
    return  {
      generatedColor: 'red',
      copiedHex: false,
      copiedRgb: false 
    }
  },
  created () {
    this.generateColor()
  },
  computed: {
    invertedColor() {
      return '#' + ("000000" + (0xFFFFFF ^ parseInt(this.generatedColor.substring(1),16)).toString(16)).slice(-6);
    },
    rgbValue() {
      const rgb = this.generatedColor.replace(/^#?([a-f\d])([a-f\d])([a-f\d])$/i
             ,(m, r, g, b) => '#' + r + r + g + g + b + b)
            .substring(1).match(/.{2}/g)
            .map(x => parseInt(x, 16))
      
      return `rgb(${rgb[0]},${rgb[1]},${rgb[2]})`
    }
  },
  methods: {
    generateColor () {
      let letters = "0123456789ABCDEF"
      let color = '#'
      for (var i = 0; i < 6; i++) {
         color += letters[(Math.floor(Math.random() * 16))]
      }
      this.generatedColor = color
    },
    copyHex () {
      if (navigator.clipboard) {
        navigator.clipboard.writeText(this.generatedColor)
      }
      this.copiedHex = true
      setTimeout(() => {
        this.copiedHex = false
      }, 500)
    },
    copyRgb () {
      if (navigator.clipboard) {
        navigator.clipboard.writeText(this.rgbValue)
      }
      this.copiedRgb = true
      setTimeout(() => {
        this.copiedRgb = false
      }, 500)
    }
  }
}
</script>
