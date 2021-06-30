<template>
  <div ref="area" :class="{ area: true, inactive: isPlaying === false }" >
    <img 
      ref="elem" 
      v-if="showImage"  
      :src="require(`../assets/elements/${elementNo}.png`)" 
      alt="element" 
      :style="{ top: top + 'px', left: left + 'px'}"
      @click="stopTimer"
    >
  </div>  
</template>

<script>
export default {
  props: ['delay', 'isPlaying', 'endGame', 'getRandomNo'],
  data() {
    return {
      elementNo:0,
      showImage: false,
      top:0,
      left:0,
      timer: null,
      reactionTime: 0
   }
  },
  mounted() {
    this.elementNo = this.getRandomNo(1, 8)
    this.$refs.area.scrollIntoView()

    let width = this.$refs.area.offsetWidth
    let height = this.$refs.area.offsetHeight

    this.left = this.getRandomNo(0, width - 150)
    this.top = this.getRandomNo(0, height - 150)

    setTimeout(() => {
      this.showImage = true
      this.startTimer()
    }, this.delay)
  }, 
  methods: {
    startTimer() {
      this.timer = setInterval(()=> {
        this.reactionTime += 10
      }, 10)
    },
    stopTimer() {
      clearInterval(this.timer)
      console.log(this.reactionTime)
      this.$emit('end', this.reactionTime)
    }
  }
}
</script>

<style scoped>
.area{
  background: #DEF2C8;
  width: 100%;
  height: 600px;
  max-height: 100%;
  padding:15px  8px;
  position: relative;
}
.area img{
  border-radius: 20px;
  position: absolute;
}
.inactive{
  background-image: url("../assets/stripes.png");
  background:#dfdfdf;
}
</style>