<template>
  <div id="timer" class="box">
      <p class="title-1">Pomodoro Timer</p>
      <button @click="startTimer">Start</button>
      <button @click="resetTimer">Reset</button>
      <button @click="pauseTimer">Pause</button>
      <button @click="completeTimer">Complete</button>
      <div id="bar" :style="{width: widthcalc + '%'}">&nbsp;</div>
      <p>{{ minutes }} : {{ seconds }}</p>
      <p>Times completed this session:</p>
      <img @click="removeOne" v-for="run in runs" :key="run" src="../assets/tomato.png" height="15">
  </div>
</template>

<script>
export default {
  name: 'Timer',
  data() {
          return {
              duration: 1500,
              runs: 0,
              countdown: 1500,
              counter: '',
              paused: false,
              startlock: false
          };
      },
      computed: {
          widthcalc() {
              return this.countdown / this.duration * 100;
          },
          minutes () {
              var minute = Math.floor(this.countdown / 60)
              if (minute < 10) {return '0' + minute.toString();}
              else {return minute;}
          },
          seconds () {
              var second = this.countdown - (Math.floor(this.countdown / 60) * 60)
              if (second < 10) {return '0' + second.toString();}
              else {return second;}
          }
      },
      methods: {
          startTimer() {
              if (this.startlock == false){
                  this.startlock = true;
                  if (this.paused == false){
                      this.countdown = this.duration;
                  }
                  var that = this;
                  this.counter = setInterval(function(){
                      that.countdown -= 1;
                      that.countdown = that.countdown.toFixed(1);
                      if (that.countdown <= 0){
                          that.paused = false;
                          that.startlock = false;
                          that.runs +=1;
                          clearInterval(that.counter);
                      }
                  }, 1000);
              }

          },
          resetTimer() {
              clearInterval(this.counter);
              this.countdown = this.duration;
              this.startlock = false;
          },
          pauseTimer() {
              clearInterval(this.counter);
              this.paused = true;
              this.startlock = false;
          },
          completeTimer() {
            this.resetTimer();
            this.runs += 1;
          },
          removeOne() {
              this.runs -= 1;
          }
      }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
    font-family: 'Roboto', sans-serif;
}

.box {
    background-color: #ddf3f1;
    border: 1px solid rgb(0, 0, 0);
    text-align: center;
    width: 300px;
    padding: 10px;
    border-radius: 5px;
}

.title-1 {
    background-color: #bc0618;
    margin: 10px 40px 10px 40px;
    padding: 5px;
    border-radius: 10px;
    color: white;
    font-weight:  bold;
    letter-spacing: 1px;
}

#bar {
    background-color: #06bca9;
    margin-top: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
}
</style>
