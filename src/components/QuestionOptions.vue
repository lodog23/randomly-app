<template>
  <div class="questionContainer">
    <!-- <ShapeAnimation></ShapeAnimation> -->
  <div class="whiteBox">

    <div id="countdown" v-if="countdownNumber !== 0 && !selectedCorrect && !selectedIncorrect">
      <div id="countdown-number">
        {{ this.countdownNumber }}
      </div>
      <svg>
        <circle r="18" cx="20" cy="20"></circle>
      </svg>
    </div>

    <div class="outOfTimeText" v-if="countdownNumber == 0">OUT OF TIME!</div>

    <h2 class="questionText" v-html="question"></h2>


    <div class="questionOptionContainer" v-for="(item, i) in options" :key="item + i">
      <button class="questionOption" :class="{'correct': selectedIncorrect || countdownNumber == 0,'buttonDisabled': countdownNumber == 0 || selectedIncorrect }" v-if="Array.isArray(item)" v-html="item[0]" :value="item[0]" @click="correctAnswerMethod($event) + $event.target.classList.add('correct')"></button>
      <button class="questionOption" :class="{'buttonDisabled': countdownNumber == 0 || selectedCorrect }" v-else v-html="item" value="0" @click="correctAnswerMethod($event) + AnsweredIncorrect() + $event.target.classList.add('incorrect')"></button>
    </div>
  </div>

  <button class="secondary-btn secondary-btn--large" v-if="selectedCorrect || selectedIncorrect || countdownNumber == 0" @click="nextTurn() + nextPoints() + getNewQuestions()">
    NEXT TURN
  </button>
</div>

</template>

<script>
export default {
  name: 'QuestionOptions',
  props: {
    options: '',
    question: '',
    correctAnswer: '',
    difficultyLevel: ''
  },

  data() {
    return {
      selectedIncorrect: false,
      selectedCorrect: false,
      pointsToEarn: 0,
      countdownNumber: 15
    }
  },

  created: function () {

    if(this.difficultyLevel == "easy"){
      console.log('this is easy');
      this.pointsToEarn = 1;
    }else if(this.difficultyLevel == "medium"){
      console.log('this is medium');
      this.pointsToEarn = 3;
    }else if(this.difficultyLevel == "hard"){
      console.log('this is hard');
      this.pointsToEarn = 5;
    }

      const self = this;
        setInterval(function(){
          if(self.countdownNumber > 0 && !self.selectedCorrect && !self.selectedIncorrect){
            self.countdownNumber = self.countdownNumber - 1;
          }
        }, 1000);


  },

  methods: {
    correctAnswerMethod: function(e) {

        var buttonValue = e.target.value;

        if(buttonValue == this.correctAnswer){
          console.log('correct answer!');
          this.selectedCorrect = true;
        }else{
          console.log('wrong answer!');
          this.selectedIncorrect = true;
        }
    },
    nextTurn(event){
      this.selectedCorrect = false;
      this.selectedIncorrect = false;

      if(this.countdownNumber == 0){
        this.pointsToEarn = 0;
      }

      this.$emit('clicked', 999);
    },

    nextPoints(event){
      this.$emit('points', this.pointsToEarn);
    },

    AnsweredIncorrect(){
      if(this.selectedIncorrect == true){
        this.pointsToEarn = 0;
      }
    },

    getNewQuestions(){
      this.$parent.getNewQuestions();
    }


  }
}
</script>

<style scoped lang="scss">

.questionContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.questionText {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 18px;
  line-height: normal;
  text-align: center;
  color: #252525;
  margin: 0 0 40px 0;
}

.questionOption {
  background: #FFFFFF;
  border: 2px solid #D8D8D8;
  box-sizing: border-box;
  border-radius: 50px;
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 16px;
  line-height: normal;
  text-align: center;
  padding: 10px;
  width: 100%;
  color: #2F2F2F;
  display: block;
  margin-bottom: 20px;
  cursor: pointer;
}

.questionOptionContainer:last-child .questionOption {
  margin-bottom: 0;
}


.correct {
  background: #01FF8D;
  border: 2px solid #01FF8D;
}

.incorrect {
  background: #FF0050;
  border: 2px solid #FF0050;
  color: #FFFFFF;
}

.buttonDisabled {
  pointer-events: none;
  cursor: not-allowed;
}

.outOfTimeText {
  color: #FF0050;
  margin-bottom: 20px;
  font-family: Montserrat;
  font-weight: 600;
  font-size: 16px;
}

#countdown {
  position: relative;
  margin: auto;
  height: 40px;
  width: 40px;
  text-align: center;
  margin-bottom: 40px;
}

#countdown-number {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  line-height: normal;
  color: #2A0D9F;
  display: inline-block;
  line-height: 40px;
}

svg {
  position: absolute;
  top: 0;
  right: 0;
  width: 40px;
  height: 40px;
  transform: rotateY(-180deg) rotateZ(-90deg);
}

svg circle {
  stroke-dasharray: 113px;
  stroke-dashoffset: 0px;
  stroke-linecap: round;
  stroke-width: 2px;
  stroke: #2A0D9F;
  fill: none;
  animation: countdown 15s linear infinite forwards;
}

@keyframes countdown {
  from {
    stroke-dashoffset: 0px;
  }
  to {
    stroke-dashoffset: 113px;
  }
}


</style>
