<template>
  <img class="logo" alt="Reaction Timer logo" src="./assets/clock.png">
  <h1>Reaction Timer</h1>
  <div> 
    <button  @click="start">start</button>
    <Block v-if="isPlaying" :delay="delay" :isPlaying="isPlaying" :getRandomNo="getRandomNo" @end="endGame" />
    <Results v-if="showResults" :score="score" />
  </div>
</template>

<script>
import Block from './components/Block.vue';
import Results from './components/Results.vue';

export default {
  name: 'App',
  components: { Block, Results },
  data() {
    return {
      isPlaying: false,      
      delay: null,
      score: null,
      showResults: false,
    }
  },
  methods: {
    getRandomNo(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    start() {
      this.delay = this.getRandomNo(500, 5000);
      this.isPlaying = true;  
      this.showResults = false
    },
    endGame(reactionTime) {
      this.score = reactionTime
      this.isPlaying = false
      this.showResults = true
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #444;
  margin-top: 60px;
}
.logo {
  width: 100%;
  max-width: 100px;
}
.feedback {
 display: flex;
 flex-direction: column;
 justify-content: center;
 align-items: center;
 padding: 8px;
}
button {
  width: 100%;
  background: #019D86;
  color: aliceblue;
  border: none;
  padding: 5px 15px;
  font-variant: small-caps;
  font-size: 18px;
  letter-spacing: 1px;
}
button:hover, button:active, button:focus{
  background: #01cbad;
  color: #fff;
}

/*info button element*/
.info{
  display: inline-block;
  position: absolute;
  top: 20px;
  left: 20px;
  width: 20px;
  height: 20px;
  background:#A8E0FF;
  color: #F7567C;
  border-radius: 50%;
  border: 0.5px solid #3E517A;
}
.info .instructions {
  position: absolute;
  visibility: hidden;
  width: 220px;
  background-color: #B08EA2;
  color: #fff;
  border-radius: 5px;
  z-index: 1;
  padding: 5px;
  opacity: 0.95;
}
.info:hover .instructions {
  visibility: visible;
}
</style>
