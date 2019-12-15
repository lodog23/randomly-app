<template>
  <div id="app">

    <div class="container">

    <!--STARTING SCREEN -->
    <div class="logoContainer" v-if="playerAmount == 0 && !showRules">
      <ShapeAnimation class="startScreenShapes"></ShapeAnimation>
      <h1 class="logo" v-if="!showRules">Randomly</h1>

      <div class="startButtons">
        <div>
          <button class="primary-btn" @click="setPlayerAmount()">Start Playing</button>
          <div>
            <select class="playerAmountSelector" v-if="showPlayerAmountSelector" v-model="playerAmount" @change="stringToNumber()">
              <option selected="selected" disabled>Select Player Amount</option>
              <option value="1">One Player</option>
              <option value="2">Two Players</option>
            </select>
          </div>
        </div>
        <div>
          <button v-if="!showRules" @click="showRules = !showRules" class="secondary-btn">How To Play?</button>
        </div>
      </div>
    </div>

    <!--SELECT A CATEGORY-->
    <ShapeAnimation v-if="playerAmount > 0"></ShapeAnimation>
    <div class="whiteBox" v-if="selected_category > 3 && !showRules && playerAmount > 0 && P1score < 10 && P2score < 10">
      <h3 class="selectCategoryText">Select a category</h3>
      <div v-if="selected_category > 3 && !showRules && playerAmount > 0" v-for="(q,i) in questions" :key="q.question">
        <button class="questionBtn" @click="chosenCategory(i) + nextRound()">
          <span class="questionBtn__difficulty" v-html="q.difficulty"></span>
          <span class="questionBtn__category" v-html="q.category"></span>
        </button>
      </div>
    </div>

    <!--OPTIONS TO PICK CORRECT ANSWER-->
    <QuestionOptions
    v-if="selected_category <= 3"
    :correctAnswer="questions[this.selected_category].correct_answer"
    :question="questions[this.selected_category].question"
    :options="questions[this.selected_category].answers"
    :difficultyLevel="questions[this.selected_category].difficulty"
    @clicked="nextTurnClicked"
    @points="nextTurnPoints"
    >
  </QuestionOptions>


  <!--HOW TO PLAY-->
  <Rules v-if="showRules"></Rules>
  <div class="startButtons" v-if="showRules">
    <div>
      <button class="primary-btn" @click="setPlayerAmount()">Start Playing</button>
      <div>
        <select class="playerAmountSelector" v-if="showPlayerAmountSelector" v-model="playerAmount" @change="stringToNumber()">
          <option selected="selected" disabled>Select Player Amount</option>
          <option value="1">One Player</option>
          <option value="2">Two Players</option>
        </select>
      </div>
    </div>
  </div>



  <!--FINAL SCORE-->
  <FinalScore v-if="P1score >= 10 || P2score >= 10"  :PlayerOne="P1score" :PlayerTwo="P2score"></FinalScore>
  <button v-if="P1score >= 10 || P2score >= 10" class="primary-btn primary-btn--paddingTop" @click="resetGame()">Play Again?</button>

  </div>

  </div>
</template>

<script>

import QuestionOptions from './components/QuestionOptions.vue'
import Rules from './components/Rules.vue'
import FinalScore from './components/FinalScore.vue'
import ShapeAnimation from './components/ShapeAnimation.vue'


export default {
  name: 'app',

  data: function() {
    return {
      rawData: [],
      selected_category: 999,
      showRules: false,
      showPlayerAmountSelector: false,

      P1score: 0,
      P2score: 0,
      P3score: 0,
      P4score: 0,

      playerAmount: 0,
      round: 0
    }

  },

  components: {
    QuestionOptions,
    Rules,
    FinalScore,
    ShapeAnimation
  },

  created: function () {
    const axios = require('axios');

    // Make a request for a user with a given ID
    axios.get(`https://opentdb.com/api.php?amount=4&type=multiple`)
      .then((response) => {
        console.log(response);
        console.log(response.data.results);
        this.rawData = response.data.results;
      })

      .catch(function (error) {
        // handle error
        console.log(error);
      })
      .then(function () {
        // always executed
      });
  },

  methods: {

    chosenCategory: function(e) {
      this.selected_category = e;
    },

    nextTurnClicked(value){
      this.selected_category = value;
    },

    nextTurnPoints(value){
      if(this.playerAmount == 1){
        console.log('1 player!');
        this.P1score = this.P1score + value;
      }else if(this.playerAmount == 2){
        console.log('2 players!');
        if (this.round % 2 === 0){
            this.P2score = this.P2score + value;
        } else {
          this.P1score = this.P1score + value;
        }
      }
      // else if(this.playerAmount == 3){
      //   console.log('3 players!');
      //   if(this.round == 1 || this.round - (this.round - 3) == 1){
      //     this.P1score = this.P1score + value;
      //   }else if(this.round == 2 || this.round - (this.round - 3) == 2){
      //     this.P2score = this.P2score + value;
      //   }else if(this.round == 3 || this.round - (this.round - 3) == 3){
      //     this.P3score = this.P3score + value;
      //   }
      // }

    },

    nextRound() {
      this.round = this.round + 1;
      this.roundMinusOne = this.roundMinusOne + 1;
      this.roundMinusTwo = this.roundMinusTwo + 1;
      this.roundMinusThree = this.roundMinusThree + 1;
    },

    getNewQuestions() {
      const axios = require('axios');
      this.rawData = [];
      // Make a request for a user with a given ID
      axios.get(`https://opentdb.com/api.php?amount=4&type=multiple`)
        .then((response) => {
          console.log(response);
          console.log(response.data.results);
          this.rawData = response.data.results;
        })

        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .then(function () {
          // always executed
        });
    },


    stringToNumber() {
      this.playerAmount = Number(this.playerAmount);
      this.showRules = false;
    },

    setPlayerAmount() {
      this.showPlayerAmountSelector = true;
    },

    resetGame() {
      this.P1score = 0;
      this.P2score = 0;
      this.playerAmount = 0;
      this.round = 0;
      this.selected_category = 999;
      this.showPlayerAmountSelector = false;
      this.showRules = false;
    }


  },

  computed: {

    questions () {
      return this.rawData.map(x => {
        const answers = [[x.correct_answer, true], ...x.incorrect_answers]
        x.answers = answers.sort((a, b) => Math.random() > .5 ? -1 : 1);
        this.$set(x)
        return x
      })
    }

  }

}
</script>

<style lang="scss">

@import url('https://fonts.googleapis.com/css?family=Montserrat:400,600,700,900');


.logoContainer {
  margin-top: 140px;
}

.logo {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 900;
  font-size: 48px;
  line-height: normal;
  color: #FFFFFF;
  text-transform: uppercase;
  // margin-bottom: 65%;
}

.startScreenShapes {
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%);
  top: 45px !important;
}

.primary-btn {
  background: #FFFFFF;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 50px;
  color: #2A0D9F;
  text-transform: uppercase;
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  line-height: normal;
  padding: 15px 50px;
  cursor: pointer;
  border: none;
  margin-bottom: 25px;
  min-width: 215px;
}

.primary-btn--paddingTop {
  margin-top: 40px;
}

.secondary-btn {
  background: #12073A;
  border-radius: 50px;
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 12px;
  line-height: normal;
  color: #FFFFFF;
  padding: 10px 20px;
  text-transform: uppercase;
  border: none;
  cursor: pointer;
}

.secondary-btn--large {
  font-size: 14px;
  min-width: 215px;
  text-align: center;
  padding: 15px 50px;
  margin-top: 40px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #FFFFFF;
  background-color: #2A0D9F;
  // padding-top: 60px;
  width: 375px;
  min-height: 650px;
  margin: 0 auto;
  position: relative;
  overflow: hidden;
}

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  padding-top: 60px;
  padding-bottom: 80px;
}

.startButtons {
  position: absolute;
  bottom: 55px;
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%)
}

.whiteBox {
  background-color: #FFFFFF;
  box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
  margin: 0 30px;
  width: 100%;
  padding: 40px 15px;
  min-width: 285px;
}



.playerAmountSelector {
  background-color: #FFFFFF;
  width: 100%;
  height: 30px;
  margin-bottom: 25px;
}


.questionBtn {
  width: 100%;
  text-align: center;
  margin: 0 auto 25px auto;
  padding: 15px;
  background: #2A0D9F;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
  border: none;
  cursor: pointer;

  .questionBtn__difficulty {
    display: block;
    font-size: 14px;
    text-transform: uppercase;
    font-family: Montserrat;
    font-style: normal;
    font-weight: 600;
    font-size: 12px;
    line-height: normal;
    color: #876EEB;
    margin-bottom: 10px;

  }

  .questionBtn__category {
    display: block;
    color: #FFFFFF;
    font-family: Montserrat;
    font-style: normal;
    font-weight: 600;
    font-size: 16px;
    line-height: normal;
  }

}

.selectCategoryText {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  line-height: normal;
  color: #7D7D7D;
  text-align: center;
  margin: 0 0 15px 0;
}

.questionSelect {
  .questionSelect__correct {
    background-color: green;
    color: white;
  }

  .questionSelect__incorrect {
    background-color: red;
    color: white;
  }
}

</style>
