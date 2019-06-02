<template>
  <div id="app">
    <div class="question-container" v-if="displayQuestions">
      <h2>{{ questions[questionIndex].text }}</h2>

      <button
        class="btn btn-answer"
        :class="{ 'selected': isSelected === index }"
        v-for="(answer, index) in questions[questionIndex].answers"
        :key="answer.index"
        :value="answer"
        v-on:change="enableButton"
        @click.prevent="selectedAnswer($event, index)"
      >{{ answer }}</button>

      <button
        v-if="!endOfQuiz"
        class="btn btn-next"
        @click="nextQuestion"
        :disabled="isDisabled"
      >Next</button>
      <button
        v-if="endOfQuiz"
        class="btn btn-next"
        @click="nextQuestion"
        :disabled="isDisabled"
      >View Summary</button>
    </div>

    <div class="summary-container" v-if="displaySummary">
      <h1>Summary:</h1>
      <div v-for="answer in userAnswers" :key="answer.question">
        <h3>{{ answer.question }}</h3>
        <p>{{ answer.answer }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      displayQuestions: true,
      displaySummary: false,
      userAnswers: [],
      answerValue: "",
      questionIndex: 0,
      isDisabled: true,
      isSelected: undefined
    };
  },
  methods: {
    enableButton: function() {
      this.isDisabled = false;
    },
    selectedAnswer: function(event, index) {
      this.isDisabled = false;
      this.clickedAnswer = event.currentTarget;
      this.answerValue = this.clickedAnswer.value;
      this.isSelected = index;
    },
    nextQuestion: function() {
      this.isDisabled = true;
      this.isSelected = undefined;

      this.userAnswers.push({
        question: this.questions[this.questionIndex].text,
        answer: this.answerValue
      });

      if (!this.endOfQuiz) {
        this.questionIndex++;
        this.answerValue = "";
      } else {
        this.displaySummary = true;
        this.displayQuestions = false;
      }
    }
  },
  computed: {
    endOfQuiz: function() {
      return this.questionIndex === this.questions.length - 1;
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Open+Sans:400,700&display=swap");
$body-font: "Open Sans", sans-serif;
$teal: #00B3AB;
$teal__dark: #234652;
$white: #FCFCFC;
$gray__dark: #333333;
$gray__medium: #999999;
$gray__light: #DCDCDC;

body {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-family: $body-font;
  color: $gray__dark;
  background-color: $gray__light;
  height: 100vh;
  margin: 0;
}

h1,
h2 {
  color: $teal__dark;
  font-weight: 700;
}

.question-container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;

  button {
    &:last-child {
      align-self: flex-end;
    }
  }
}

.btn {
  padding: 8px 14px;
  font-size: 1em;
  background: none;
  line-height: 1.5;
  cursor: pointer;

  &:disabled {
    cursor: not-allowed;
    border: 2px solid rgba($gray__medium, 0.4);
    color: rgba($gray__medium, 0.4);

    &:hover {
      background-color: transparent;
      color: rgba($gray__medium, 0.4);
    }
  }

  &-next,
  &-answer {
    transition: all 0.3s ease-in-out;
    letter-spacing: 0;
    border-radius: 15px;
    letter-spacing: 1px;
    font-weight: bold;
  }

  &-next {
    border: 2px solid $teal__dark;
    color: $teal;

    &:active,
    &:focus,
    &:hover {
      color: $white;
      background-color: $teal__dark;
    }
  }

  &-answer {
    border: 2px solid $teal;
    color: $gray__dark;
    width: 100%;
    margin-bottom: 1.2em;

    &:active,
    &:focus,
    &:hover,
    &.selected {
      color: $white;
      background-color: $teal;
    }
  }
}
</style>