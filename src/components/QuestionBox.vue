<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot:lead>
        Book titles for
        <b>{{ currentBook.author }}</b>
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(book, index) in shuffledAnswers"
          :key="book"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ book }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button variant="success" @click="next">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import lodash from 'lodash'

export default {
  props: {
    currentBook: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentBook.title]
      answers.push(this.currentBook.correct_title)
      return answers
    }
  },
  watch: {
    currentBook: {
      immediate: true, //Forces to execute the handler when the prop currentBook is first created, then after it is modified
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers()
        this.answered = false
      }
    }
  },
  methods: {
    submitAnswer() {
      let isCorrect = false
      // console.log('submit')
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.increment(isCorrect)
      this.answered = true
    },
    selectAnswer(index) {
      console.log(index);
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentBook.title, this.currentBook.correct_title]
      this.shuffledAnswers = lodash.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentBook.correct_title)
    },
    answerClass(index) {
      let answerClass = ''
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.selectedIndex === index && this.answered && this.correctIndex !== index) {
        answerClass = 'incorrect'
      }
      return answerClass
    }
  },
  mounted() {
    console.log("answers: " + this.answers)
  }
};
</script>

<style scoped>
/* Scoped styles only affect this Vue component */
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>