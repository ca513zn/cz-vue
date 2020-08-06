<template>
  <div class="questionBox">
    <b-jumbotron>
      <template v-slot:lead>{{currQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group class="questionBox__listGroup">
        <b-list-group-item
          v-for="(answer, idx) in shuffledAnswers"
          :key="idx"
          @click="selectAnwser(idx)"
          :class="answerClass(idx)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        :disabled="selectedIdx === null || answered"
        variant="primary"
        @click="submitAnswer"
      >Submit</b-button>
      <b-button class="questionBox__next" variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>


<script>
import _ from "lodash";
export default {
  props: {
    currQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIdx: null,
      correctIdx: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  //watches for changes and re-renders dependent components
  watch: {
    currQuestion: {
      //will run the funciton the first time the component
      //is mounted as opposed to clicking next
      immediate: true,
      handler() {
        this.selectedIdx = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnwser(idx) {
      this.selectedIdx = idx;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIdx === this.correctIdx) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [
        ...this.currQuestion.incorrect_answers,
        this.currQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIdx = this.shuffledAnswers.indexOf(
        this.currQuestion.correct_answer
      );
    },
    answerClass(idx) {
      let answerClass = "";
      if (!this.answered && this.selectedIdx === idx) {
        answerClass = "selected";
      } else if (this.answered && this.correctIdx === idx) {
        answerClass = "correct";
      } else if (this.answered && this.selectedIdx === idx && this.correctIdx !== idx) {
        answerClass = "incorrect";
      }
      return answerClass 
    }
  },
  computed: {
    answers() {
      let answers = [...this.currQuestion.incorrect_answers];
      answers.push(this.currQuestion.correct_answer);
      return answers;
    }
  }
};
</script>


<style scoped>
.questionBox__listGroup {
  margin-bottom: 15px;
}
.selected {
  background-color: skyblue;
}
.correct {
  background-color: springgreen;
}
.incorrect {
  background-color: red;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.questionBox__next {
  margin: 0 5px;
}
</style>