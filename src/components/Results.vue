<template>
  <p class="blue">Reaction time: {{ score }}</p>
  <p class="rank">{{rank}}</p>
</template>

<script>
export default {
props: ['score'],
data() {
  return{
    rank: null
  }
},
mounted() {
  if (this.score < 500) {
    this.rank = 'Finger master! 🖱️💨'
  } else if (this.score < 700) {
    this.rank = 'Rapid reflexes 🚗'
  } else {
    this.rank = 'Snail pace...🐌'
  }
}
}
</script>

<style>
.blue{
  font-size: 1.2em;
  padding: 5px;
  margin-top: 20px;
}
.rank{
  font-size: 1.4em;
  color: #0faf87;
  font-weight: bold;
}
</style>