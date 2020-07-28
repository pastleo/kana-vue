<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h2>Vue 3 using Composition API</h2>
    <KanaVue3 initMsg="Welcome to the dead simple Hiragana/Katakana practice using Composition API"/>
    <hr>
    <KanaVue2 initMsg="Original dead simple Hiragana/Katakana practice"/>
    <hr>
    <Counter />
    <p><a href="https://en.wikipedia.org/wiki/Goj%C5%ABon#Table" target="_blank">Kana Table</a></p>
    <p><a href="https://github.com/pastleo/kana-vue" target="_blank">Source Code</a></p>
  </div>
</template>

<script>
import KanaVue2 from './components/KanaVue2.vue'
import KanaVue3 from './components/KanaVue3.vue'
import Counter from './components/Counter.vue'

export default {
  name: 'App',
  components: { KanaVue2, KanaVue3, Counter }
}
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
