<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row mt-3">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="books.length"
            :currentBook="books[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      books: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    };
  },
  methods: {
    getBooks: function() {
      const path = "http://localhost:5000/books";
      axios
        .get(path)
        .then(res => {
          this.books = res.data.books;
          console.log("Received: " + JSON.stringify(this.books));
        })
        .catch(error => {
          console.error(error);
        });
    },
    next() {
      if(this.index < 2) {
        this.index++
      }
      else {
        this.index = 0
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      // console.log('Increment+')
      this.numTotal++
    }
  },
  created: function() {
    this.getBooks();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
