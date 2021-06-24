<template>
  <div class="hello"> 
    <div class="feedback">
      <p v-if="!gameOngoing">{{ instructions }}</p>  
    </div>
    <button v-if="!gameOngoing" @click="startGame">{{ buttonText}}</button>
    <div ref="area" :class="{ area: true, inactive: gameOngoing === false }" @mousedown="endGame">
      <img ref="elem" src="../assets/elements/apple.png" alt="element">
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'Details',
  data() {
    return {
      instructions: "Try to click on the appearing elements as fast as possible but watch out for bombs. The faster you are, the higher score you get.",
      buttonText: 'start',
      gameOngoing: false,
      delay: 0,
      startTime: 0,
      endTime: 0,
      timer: null
    }
  },
  methods: {
    getRandomArbitrary(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    startGame() {
      this.$refs.area.scrollIntoView();
      this.gameOngoing = true;  
      this.startTime = Date.now();
      this.showImage();    
    },
    showImage() {
      this.$refs.elem.style.visibility = 'hidden';
      this.$refs.elem.style.opacity = '1';
      const delay = this.getRandomArbitrary(500, 3000);
      this.delay = delay;
      const y = this.getRandomArbitrary(0, (this.$refs.area.clientWidth - 158));
      const x = this.getRandomArbitrary(0, (this.$refs.area.clientHeight - 158));
      this.$refs.elem.style.top =  x + 'px';
      this.$refs.elem.style.setProperty('left', y + 'px');
      console.log(x,y, delay);
      this.timer = setTimeout(() => {   
        this.$refs.elem.style.visibility = 'visible';
      }, delay);
    },
    endGame(e) {
      if (this.gameOngoing) {
        this.endTime = Date.now();
        this.gameOngoing = false;
        this.$refs.elem.style.opacity = '0.4';
        const userTime = this.endTime - this.delay - this.startTime;
        if ((this.endTime - this.startTime) > this.delay) {
          if(e.target.tagName === 'IMG') {

            switch(true) {
              case (userTime < 301):
                this.instructions = `🏆 Congratulations! You were super fast 🌠. You managed to hit the target within ${userTime} miliseconds`;
                break;
              case (userTime < 601):
                this.instructions = `🏆 Great job! You were very fast 🏂. You managed to hit the target within ${userTime} miliseconds`;
                break;
              case (userTime < 801):
                this.instructions = `Good job! You were not in a hurry 🚶 but still hit the target within ${userTime} miliseconds`;
                break;
              case (userTime < 1001):
                this.instructions = `You can do better! Try to 🏃💨 and hit the target faster than ${userTime} miliseconds`;
                break;
              case (userTime >= 1001):
                this.instructions = `Oh no! The 🐌 was faster. You only got ${userTime} miliseconds`;
                break;
            }            
            this.buttonText = '⟲ try again';
            
          } else {
            this.instructions = `You missed! This time you missed but maybe next time you will get it!`;
            this.buttonText = '⟲ try again';
          };
          
        } else {
          this.instructions = 'Too soon! You clicked before the element appeared.';
          this.buttonText = '⟲ try again';
          clearTimeout(this.timer);
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.feedback {
 height:60px;
 display: flex;
 flex-direction: column;
 justify-content: center;
 align-items: center;
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
.area{
  width: 100%;
  min-height: 600px;
  background:#dfdfdf;
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
  /* background-position: center center;
  background-repeat: no-repeat; */
}
</style>
