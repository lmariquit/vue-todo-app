<template>
  <div id="app">
    <Header/>
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Header from '../components/layout/Header'
import Todos from '../components/Todos'
import AddTodo from '../components/AddTodo'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Todos,
    Header,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    async deleteTodo(id) {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        this.todos = this.todos.filter(todo => todo.id !== id)
      } catch (err) {
        // eslint-disable-next-line
        console.log('There has been an error with deleteTodo --> ', err)
      }
    },
    async addTodo(newTodo) {
      const { title, completed } = newTodo
      try {
        const res = await axios.post(
          'https://jsonplaceholder.typicode.com/todos',
          {
            title,
            completed
          }
        )
        this.todos = [...this.todos, res.data]
      } catch (err) {
        // eslint-disable-next-line
        console.log('There has been an error with addTodo --> ', err)
      }
    }
  },
  async created() {
    try {
      const res = await axios.get(
        'https://jsonplaceholder.typicode.com/todos?_limit=10'
      )
      this.todos = res.data
    } catch (err) {
      // eslint-disable-next-line
      console.log('There has been an error with created --> ', err)
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
