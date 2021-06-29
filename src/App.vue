<template>
  <img class="logo" alt="Reaction Timer logo" src="./assets/clock.png">
  <h1>Reaction Timer</h1>
  <div class="hello"> 
    <div class="feedback" ref="response">
      <p v-if="!isPlaying">
        <span>{{ feedback }}</span>
        <span v-if="showInstructions">{{ instructions }}</span>
        <span v-if="!showInstructions" class="info"> ℹ️ 
          <span class="instructions">Try to click on the appearing element as fast as possible but watch out for bombs. If you see a bomb, click around it as fast as possible and wait for the next element to appear.</span>
        </span>         
      </p>  
    </div>
    <button ref="btn" v-if="!isPlaying" @click="start">{{ buttonText}}</button>
    <Block ref="area" :delay="delay" :isPlaying="isPlaying" :endGame="endGame" :elementNo="elementNo" />
  </div>
</template>

<script>
import Block from './components/Block.vue';

export default {
  name: 'App',
  components: { Block },
  data() {
    return {
      isPlaying: false,      
      delay: 0,
      instructions: "Try to click on the appearing element as fast as possible but watch out for bombs. If you see a bomb, click around it as fast as possible and wait for the next element to appear.",
      showInstructions: true,
      feedback: "",
      buttonText: 'start',
      elementNo: 1,
      startTime: 0,
      endTime: 0,
      timer: null
    }
  },
  methods: {
    getRandomArbitrary(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    start() {
      this.elementNo = this.getRandomArbitrary(1,8);
      this.showInstructions = false;
      this.$refs.area.scrollIntoView({behavior: "smooth", block: "center"});
      console.log(this.$refs.btn);
      this.isPlaying = true;  
      this.startTime = Date.now();
      this.showImage();    
    },
    showImage() {
      this.$refs.elem.style.visibility = 'hidden';
      this.$refs.elem.style.opacity = '1';
      this.delay = this.getRandomArbitrary(500, 3000);
      const y = this.getRandomArbitrary(0, (this.$refs.area.clientWidth - 158));
      const x = this.getRandomArbitrary(0, (this.$refs.area.clientHeight - 158));
      this.$refs.elem.style.top =  x + 'px';
      this.$refs.elem.style.setProperty('left', y + 'px');
      console.log(x,y, this.delay);
      this.timer = setTimeout(() => {   
        this.$refs.elem.style.visibility = 'visible';
      }, this.delay);
    },
    endGame(e) {
      if (this.isPlaying) {
        
        this.endTime = Date.now();
        this.isPlaying = false;
        this.$refs.elem.style.opacity = '0.4';
        this.$refs.response.scrollIntoView({behavior: "smooth", block: "center"});

        const userTime = this.endTime - this.delay - this.startTime;
        if ((this.endTime - this.startTime) > this.delay) {
          if(e.target.tagName === 'IMG') {
            if(this.elementNo === 3 || this.elementNo === 4) {
               this.feedback = `💥💔 You lost! You just clicked on a 💣 within ${userTime} miliseconds`;
            } else {
              switch(true) {
                case (userTime < 301):
                  this.feedback = `🏆 Congratulations! You were super fast 🌠. You managed to hit the target within ${userTime} miliseconds`;
                  break;
                case (userTime < 601):
                  this.feedback = `🏆 Great job! You were very fast 🏂. You managed to hit the target within ${userTime} miliseconds`;
                  break;
                case (userTime < 801):
                  this.feedback = `Good job! You were not in a hurry 🚶 but still hit the target within ${userTime} miliseconds`;
                  break;
                case (userTime < 1001):
                  this.feedback = `You can do better! Try to 🏃💨 and hit the target faster than ${userTime} miliseconds`;
                  break;
                case (userTime >= 1001):
                  this.feedback = `Oh no! The 🐌 was faster. You only got ${userTime} miliseconds`;
                  break;
              } 
            }                       
          } else {
            if(this.elementNo === 3 || this.elementNo === 4) {
              this.start();
            } else {
              this.feedback = `You missed! This time you missed but maybe next time you will get it!`;
            }
          };
          
        } else {
          this.feedback = 'Too soon! You clicked before the element appeared.';          
          clearTimeout(this.timer);
        }
        this.buttonText = '⟲ try again';
      }
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
