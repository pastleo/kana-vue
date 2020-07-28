<template>
  <div class="hello">
    <h2>{{ msg }}</h2>
    <p>{{ submsg }}</p>

    <div class="input">
      <p v-if="started">What is the sound of the symbol below?</p>
      <p v-else>Enter the sound of the symbol below to start:</p>
      <p>{{ question }}</p>
      <input v-model="answer" v-on:keyup.enter="submit" placeholder="input answer and press enter">
      <div v-if="started">
        <p>Socre: {{ score }}</p>
        <p>Time Used: {{ msPassed / 1000 }} seconds</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { kana } from '../lib/kana.json';

export default {
  name: 'KanaVue3',
  props: {
    initMsg: String,
  },
  setup(props) {
    const answer = ref('');
    const latestMsg = ref('');
    const submsg = ref('');
    const question = ref('');
    const score = ref(0);
    const started = ref(false);
    const msPassed = ref(0);

    let correctWrongs = {};
    let questionIndex = 0;
    let msPassedTimer = null;

    const nextQuestion = () => {
      questionIndex = Math.floor(kana.length * Math.random());
      question.value = kana[questionIndex][0];
      answer.value = '';
    }

    const submit = () => {
      if (answer.value === '') return;
      const isCorrect = answer.value === kana[questionIndex][1];

      if (isCorrect) {
        latestMsg.value = 'Correct Answer!';
        submsg.value = `'${question.value}' is '${answer.value}'`;
      } else {
        latestMsg.value = 'Wrong!';
        submsg.value = `'${question.value}' is not '${answer.value}', should be '${kana[questionIndex][1]}'`;
      }

      if (started.value) {
        if (correctWrongs[questionIndex] === undefined) {
          correctWrongs[questionIndex] = 0;
        }
        if (isCorrect) {
          score.value++;
          correctWrongs[questionIndex]++;
        } else {
          score.value--;
          correctWrongs[questionIndex]--;
        }

        if (correctWrongs[questionIndex] >= 0) {
          submsg.value += `, this symbol is correct for ${correctWrongs[questionIndex]} times`;
        } else {
          submsg.value += `, this symbol is wrong for ${-correctWrongs[questionIndex]} times`;
        }
      } else if (isCorrect) {
        started.value = true;
        msPassedTimer = setInterval(() => {
          msPassed.value += 100;
        }, 100);
      }

      nextQuestion();
    }

    onMounted(nextQuestion);
    onUnmounted(() => {
      clearInterval(msPassedTimer);
    });

    return {
      answer, submsg, question, score, started, msPassed,
      msg: computed(() => latestMsg.value || props.initMsg),
      submit,
    }
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
