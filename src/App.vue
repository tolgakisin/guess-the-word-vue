<template>
  <div id="app">
    <div class="container mt-4">
      <div class="card">
        <div class="card-content">
          <div class="card-header">
            <div style="text-align:center;font-weight:bold;">GUESS THE WORD</div>
          </div>
          <div class="card-body" v-if="!isStarted">
            <div v-if="!isFinished">
              <div
                style="text-align:center;"
              >Welcome to our game. If you want to have fun, you can click the button below.</div>
              <div style="text-align:center;">Have a good time!</div>
            </div>
            <div v-if="isFinished">
              <div style="text-align:center;">Congratulations! Your score : {{totalScore}}</div>
            </div>
          </div>
        </div>
      </div>
      <div class="start mt-2" v-if="!isStarted">
        <button type="submit" class="btn btn-primary" v-on:click="start">Start the game</button>
      </div>

      <div v-if="isStarted">
        <div class="card mt-2">
          <div class="card-header">
            <span>{{currentQuestion.question}}</span>
          </div>
          <div class="card-body">
            <div class="d-flex" style>
              <Letter :value="letter.letter" :open="letter.isOpened" v-for="letter in letters" v-bind:key="letter.index" />
            </div>
          </div>
          <div class="card-footer">
            <div class="d-flex">
              <div class="mr-4">Total Score : {{totalScore}}</div>
              <div class="mr-4">
                Remaining Time :
                <kbd>{{remainingTime}}</kbd>
              </div>
              <div>Score Of Question : {{scoreOfQuestion}}</div>
            </div>
          </div>

          <div class="card-footer">
            <div class="input-group">
              <input class="form-control" v-model="usersAnswer" placeholder="Your Answer" />
              <div class="input-group-append">
                <button
                  type="submit"
                  class="btn btn-primary ml-2"
                  v-on:click="showLetter"
                >Give me a letter</button>
                <button
                  type="submit"
                  class="btn btn-primary ml-2"
                  v-on:click="answer"
                >Send this answer</button>
              </div>
            </div>
          </div>
          <div class="card-body" v-if="message">
            <div class="alert" :class="messageClass" role="alert">{{message}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { setInterval, clearInterval } from "timers";
import Letter from "@/components/Letter";

export default {
  name: "app",
  components: {
    Letter
  },
  data: () => {
    return {
      questions: [
        {
          question: "Soru 11",
          answer: "ABCD",
          isAsked: false
        },
        {
          question: "Soru 22",
          answer: "ABCDE",
          isAsked: false
        },
        {
          question: "Soru 33",
          answer: "ABCDEF",
          isAsked: false
        },
        {
          question: "Soru 44",
          answer: "ABCDEFG",
          isAsked: false
        }
      ],
      totalScore: 0,
      scoreOfQuestion: 0,
      letters: [],
      currentQuestion: null,
      isStarted: false,
      usersAnswer: "",
      time: null,
      remainingTime: 0,
      isFinished: false,
      message: null,
      messageClass: ""
    };
  },
  methods: {
    start() {
      this.totalScore = 0;
      this.scoreOfQuestion = 0;
      this.message = null;
      this.messageClass = "";
      this.isFinished = false;
      this.isStarted = true;
      this.questions.map(x => {
        x.isAsked = false;
      });
      this.fillCurrentQuestion();

      this.remainingTime = 240;
      this.time = setInterval(() => {
        this.remainingTime--;

        if (this.remainingTime === 0) {
          this.finish();
        }
      }, 1000);
    },
    finish() {
      alert("Game is over");
      clearInterval(this.time);
      this.isStarted = false;

      this.letters = [];
      this.isFinished = true;
    },
    fillCurrentQuestion() {
      this.letters = [];
      this.usersAnswer = "";
      this.currentQuestion = this.questions.find(x => !x.isAsked);
      if (this.currentQuestion == null) {
        this.finish();
        return;
      }

      for (var i = 0; i < this.currentQuestion.answer.length; i++) {
        var letter = {
          letter: "",
          isOpened: false
        };
        letter.letter = this.currentQuestion.answer.split("")[i];
        this.letters.push(letter);
      }

      this.scoreOfQuestion = 100 * this.letters.length;
    },
    showLetter() {
      //var letter = this.letters.find(x=>!x.isOpened);
      if (this.letters.find(x => !x.isOpened) == null) {
        console.log("bitti");
        return;
      }
      var letterIndex = Math.floor(Math.random() * this.letters.length);
      //this.letters[letterIndex].isOpened = true;
      while (this.letters[letterIndex].isOpened === true) {
        letterIndex = Math.floor(Math.random() * this.letters.length);
      }
      this.letters[letterIndex].isOpened = true;
      console.log(letterIndex);
      // while(this.letters[letterIndex].isOpened === true){
      //   this.letters[letterIndex].isOpened = true;
      // }
      this.scoreOfQuestion -= 100;
    },
    answer() {
      if (
        this.usersAnswer.toLocaleUpperCase("tr") ===
        this.currentQuestion.answer.toLocaleUpperCase("tr")
      ) {
        this.totalScore += this.scoreOfQuestion;
        this.answerAlert(true);
      } else {
        this.totalScore -= this.scoreOfQuestion;
        this.answerAlert(false);
      }
      this.currentQuestion.isAsked = true;
      this.fillCurrentQuestion();
    },
    answerAlert(isSuccess) {
      if (isSuccess) {
        this.message = "Correct Answer!";
        this.messageClass = "alert-success";
      } else {
        this.message = "Please Check Your Answer";
        this.messageClass = "alert-danger";
      }
    }
  }
};
</script>

<style>
.start {
  display: flex;
  justify-content: center;
}
</style>
