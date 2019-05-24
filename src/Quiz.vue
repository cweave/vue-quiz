<template>
  <!-- <pre>{{ JSON.stringify(questions, null, 2) }}</pre> -->
	<div id="app">
		<div class="question-container" v-if="questionIndex < questions.length" v-bind:key="questionIndex">
			<h2>{{ questions[questionIndex].text }}</h2>

			<button
				type="button"
				class="btn btn-answer"
				:class="{ 'selected': userAnswers[questionIndex] == index }"
				v-for="(answer, index) in questions[questionIndex].answers"
				:key="index"
				@click="selectedAnswer(index);"
			>
				{{ answer }}
			</button>

			<button class="btn btn-next" :class="(userAnswers[questionIndex]==null)?'disabled':'is-active'" v-on:click="next();" :disabled="(userAnswers[questionIndex]==null)">Next</button>
		</div>
	</div>
</template>

<script>
import Vue from 'vue'
import questions from '@/questions.js'

export default {
	props: {
		questions: {
			type: Array,
			required: true
		}
	},
	data() {
		return {
			userAnswers: [],
			questionIndex: 0,
			activeAnswer: false
		};
	},
	methods: {
		selectedAnswer: function(index) {
			Vue.set(this.userAnswers, this.questionIndex, index);
		},
		next: function() {
         if (this.questionIndex < this.questions.length)
            this.questionIndex++;
      },
	}
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Inconsolata|Oswald:200,300,400,500,600,700');
$body-font: "Oswald", sans-serif;
$blue: #29ABE2;
$dark-blue: #186C90;
$white: #FFFFFF;
$dark-gray: #333333;
$medium-gray:#999999;
$light-gray: #e8e8e8;
$red:#BC091E;
$green: #288b00;


body {
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 16px;
	font-family: $body-font;
}

.question-container {
	display: flex;
    justify-content: center;
    flex-direction: column;
    width: 30rem;
    align-items: center;
}

.btn {
	padding: 8px 14px;
	font-size: 1em;
	background: none;
	line-height: 1.5;
	cursor: pointer;

	&:disabled {
		cursor: not-allowed;
		border: 2px solid rgba($medium-gray, 0.4);
		color: rgba($medium-gray, 0.4);

		&:hover {
			background-color: transparent;
			color: rgba($medium-gray, 0.4);
		}
	}

	&-next,
	&-answer {
		transition: all .3s ease-in-out;
		letter-spacing: 0;
		border-radius: 15px;
		letter-spacing: 1px;
		font-weight: bold;
	}

	&-next {
		border: 2px solid $dark-gray;
		color: $dark-blue;

		&:active,
		&:focus,
		&:hover {
			color: $white;
			background-color: $dark-gray;
		}
	}

	&-answer {
		border: 2px solid $dark-blue;
		color: $dark-gray;
		width: 100%;
		margin-bottom: 1.2em;

		&:active,
		&:focus,
		&:hover,
		&.selected {
			color: $white;
			background-color: $dark-blue;
		}
	}
}
</style>


