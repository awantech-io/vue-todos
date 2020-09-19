<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';
export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo

  },

  data(){
    return {
      todos: []
    }
  },

  methods:{

    // deleteTodo(id) {
    //   this.todos = this.todos.filter( todo => todo.id !== id );
    // },

    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res => this.todos = this.todos.filter(todo => todo.id !== id, res.data))
      .catch(err => console.log(err));
    },

    // addTodo(newTodo) {
    //   this.todos = [...this.todos, newTodo];
    // }

    addTodo(newTodo) {
      const { title, completed } = newTodo;

      // post the new todos to json api
      axios.post('https://jsonplaceholder.typicode.com/todos',
      {
        title,
        completed
      })
      .then( res => this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));
      
    }

  },

    // method for http request json placeholder
    created(){
      axios.get('https://jsonplaceholder.typicode.com/posts?_limit=5')
      .then( res => this.todos = res.data)
      .catch(err => console.log(err));
      //console.log(this.todos);
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
