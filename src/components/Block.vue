<template>
  <div :class="{ area: true, inactive: isPlaying === false }" @mousedown="endGame">
      <img ref="elem" :src="require(`../assets/elements/${elementNo}.png`)" alt="element">
    </div>  
</template>

<script>
export default {
 props: ['delay', 'isPlaying', 'endGame', 'elementNo']
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
  visibility: hidden;
}
.inactive{
  background-image: url("../assets/stripes.png");
  background:#dfdfdf;
}
</style>