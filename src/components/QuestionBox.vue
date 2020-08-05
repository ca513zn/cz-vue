<template>
  <div class="questionBox">
    <b-jumbotron>
      <template v-slot:lead>{{currQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group class="questionBox__listGroup">
        <b-list-group-item
          v-for="(answer, idx) in answers"
          :key="idx"
          @click="selectAnwser(idx)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button class="questionBox__next" variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>


<script>
export default {
  props: {
    currQuestion: Object,
    next: Function
  },
  data() {
    return {
      selectedIdx: null
    };
  },
  methods: {
    selectedAnswer(idx) {}
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
.questionBox__next {
  margin: 0 5px;
}
</style>