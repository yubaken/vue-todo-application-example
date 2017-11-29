<template>
  <div id="app">
    <Todo :items="data" :updateAt.sync="updateAt"></Todo>
    <p>Debug</p>
    <p>UpdateAt: {{updateAt}}</p>
    <p v-for="d in data">{{d}}</p>
  </div>
</template>

<script>
import Todo from './components/Todo'

export default {
  name: 'app',
  data () {
    return {
      data: [
        {id: 1, todo: '1 Todo Test', updateAt: new Date(2017, 1, 1, 0, 0, 0), checked: false},
        {id: 2, todo: '2 Todo Test', updateAt: new Date(2017, 2, 1, 0, 0, 0), checked: true},
        {id: 3, todo: '3 Todo Test', updateAt: new Date(2017, 3, 15, 0, 0, 0), checked: false},
        {id: 4, todo: '4 Todo Test', updateAt: new Date(2017, 4, 2, 0, 0, 0), checked: true},
        {id: 5, todo: '5 Todo Test', updateAt: new Date(2016, 12, 1, 0, 0, 0), checked: false},
        {id: 6, todo: '6 Todo Test', updateAt: new Date(2016, 1, 1, 0, 0, 0), checked: true}
      ],
      updateAt: new Date()
    }
  },
  components: {
    Todo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding-top: 30px;
  padding-bottom: 30px;
  background: #bfc5c9;
}
</style>
