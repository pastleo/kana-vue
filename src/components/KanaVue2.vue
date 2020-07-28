<template>
  <div class="hello">
    <h2>{{ msg }}</h2>
    <p>{{ submsg }}</p>

    <div class="input">
      <p>What is the sound of the symbol below?</p>
      <p>{{ question }}</p>
      <input v-model="answer" v-on:keyup.enter="submit" placeholder="input answer and press enter">
      <div v-if="correctWrongs.length !== 0">
        <p>Socre: {{ score }}</p>
        <p>Time Used: {{ msPassed / 1000 }} seconds</p>
      </div>
    </div>
  </div>
</template>

<script>
import { kana } from '../lib/kana.json';

export default {
  name: 'KanaVue2',
  props: {
    initMsg: String,
  },
  data () {
    return {
      msg: this.initMsg,
      submsg: '',
      question: 'just inited',
      answer: 'just inited',
      questionIndex: 0,
      score: 0,
      correctWrongs: [],
      msPassed: 0,
    }
  },
  methods: {
    submit () {
      if (this.correctWrongs[this.questionIndex] === undefined) {
        this.correctWrongs[this.questionIndex] = 0
      }
      if (this.answer === this.correctAnswer) {
        this.score++
        this.correctWrongs[this.questionIndex]++
        this.msg = 'Correct Answer!'
        this.submsg = `'${this.question}' is '${this.answer}'`
      } else {
        this.score--
        this.correctWrongs[this.questionIndex]--
        this.msg = 'Wrong!'
        this.submsg = `'${this.question}' is not '${this.answer}', should be '${this.correctAnswer}'`
      }
      if (this.correctWrongs[this.questionIndex] >= 0) {
        this.submsg += `, this symbol is correct for ${this.correctWrongs[this.questionIndex]} times`
      } else {
        this.submsg += `, this symbol is wrong for ${-this.correctWrongs[this.questionIndex]} times`
      }

      if (!this.msPassedTimer) {
        this.msPassedTimer = setInterval(() => {
          this.msPassed += 100;
        }, 100)
      }

      this.nextQuestion()
    },
    nextQuestion () {
      // debugger
      this.questionIndex =
        Math.floor(kana.length * Math.random())
      this.question = kana[this.questionIndex][0]
      this.correctAnswer = kana[this.questionIndex][1]
      this.answer = ''
    }
  },
  created () {
    this.nextQuestion()
  },
  beforeUnmoun() {
    clearInterval(this.msPassedTimer)
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.input {
  margin-bottom: 20px;
}
.input input {
  width: 200px;
  text-align: center;
}
</style>
