<template>
  <button ref="btn" @mousemove="this.magnetize" @mouseleave="this.mouseLeave" :class="{ 'cerchio' : true, 'magnet': magnetClass }">
      Start your project
  </button>

</template>

<script>
import { TweenMax } from 'gsap'
export default {
  name: 'MagnetBtn',
  data () {
    return {
      dist: 7,
      magnetClass: false
    }
  },
  methods: {
    mouseLeave () {
      const item = this.$refs.btn
      this.magnetClass = false
      TweenMax.to(item, 0.6, { y: 0, x: 0, scale: 1 })
    },
    magnetize (e) {
      const mX = e.pageX
      const mY = e.pageY

      const item = this.$refs.btn
      const customDist = Number(this.dist) * 20 || 120
      const centerX = item.offsetLeft + (item.offsetWidth / 2)
      const centerY = item.offsetTop + (item.offsetHeight / 2)
      var deltaX = Math.floor((centerX - mX)) * -0.45
      var deltaY = Math.floor((centerY - mY)) * -0.45
      var distance = this.calculateDistance(item, mX, mY)
      
      if (distance < customDist) {
        TweenMax.to(item, 0.5, { y: deltaY, x: deltaX, scale: 1.2 })
        this.magnetClass = true
      } else {
        TweenMax.to(item, 0.6, { y: 0, x: 0, scale: 1 })
        this.magnetClass = false
      }
    },
    calculateDistance (elem, mouseX, mouseY) {
      return Math.floor(Math.sqrt(Math.pow(mouseX - (elem.offsetLeft + (elem.offsetWidth / 2)), 2) + Math.pow(mouseY - (elem.offsetTop + (elem.offsetHeight / 2)), 2)))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  font-family: "Helvetica", sans-serif;
  background: black;
}

.cerchio {
  position: relative;
  top: calc(50vh - 30px);
  /* left: calc(50vw - 30px); */
  width: 160px;
  height: 160px;
  border: 1px solid rgba(255, 0, 0, 0.75);
  background: transparent;
  border-radius: 50%;
  cursor: pointer;
  outline: 0;
  color: red;
  transition: background 0.4s cubic-bezier(0.645, 0.045, 0.355, 1);
}

.cerchio:hover {
  background: rgb(187, 37, 37);
}

.cerchio.magnet {
  border: 2px solid rgb(255, 0, 0);
  color: white;
}

.cursor-wrap {
  position: fixed;
  top: 0;
  left: 0;
  pointer-events: none;
}

.cursor {
  position: fixed;
  top: -30px;
  left: -30px;
}

.circle {
  transform: scale(0.8);
  transition: all 0.2s ease-in-out;
}

</style>
