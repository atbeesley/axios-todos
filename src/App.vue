<template>
  <div id="app">
    <h1>Todos los todos</h1>
    <input type="text" v-model="todoName" @keyup.enter="addTodo">
      <ul>
        <li v-for="todo of todos" :key="todo.id">{{todo.name}}
            <button @click="deleteTodo(todo)" :key="todo.id">delete</button>
            <button v-if="!showEditBox" @click="editTodo(todo)">edit</button>
            <input v-if="showEditBox" type="text" :key="todo.id" @keyup.enter="saveChange">
        </li>
      </ul>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = "http://localhost:3000/todos";

export default {
  name: "app",
  data() {
    return {
      todos: [],
      todoName: '',
      todoId:'',
      showEditBox: Boolean
    };
  },
  async created(){
    this.showEditBox=false;
    try {
      const res = await axios.get(baseURL);
      this.todos = res.data;
    } catch(e){
      console.error(e);
    }
  },
  methods: {
    async addTodo(){
      const res = await axios.post(baseURL, { name: this.todoName});

      this.todos = [...this.todos, res.data];

      this.todoName = '';
    },
    deleteTodo(todo){
      axios.delete(baseURL+'/'+todo.id);
      window.location.reload();
     },
    editTodo (){
      this.showEditBox=true;
      console.log(this.showEditBox);
    },
    async saveChange(todo){
      const res = await axios.patch(baseURL+'/'+todo.id, { name: this.todoName});
      this.todos = [...this.todos, res.data];
      this.todoName = '';
      this.showEditBox=false;
      console.log('showEditBox: ', this.showEditBox);
      console.log('edited: ', todo);
    } 
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

ul{
  margin: 0px; 
  padding: 0px;
}

li {
  list-style: none;
}
</style>
