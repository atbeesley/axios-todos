<template>
  <div id="app">
    <h1>Todos los todos</h1>
    <input type="text" v-model="todoName" :placeholder="'enter a todo'" @keyup.enter="addTodo" style="margin-bottom: 1em">
      <ul>
        <li v-for="todo of todos" :key="todo.id"><span v-if="!showEditBox">{{todo.name}}</span>
            <button v-if="!showEditBox" @click="deleteTodo(todo)" :key="todo.id" class="btn btn-danger">delete</button>
            <button v-if="!showEditBox" :key="todo.id" @click="editTodo" class="btn btn-primary">edit</button>
            <input v-if="showEditBox" type="text" :value="todo.name" :key="todo.id" @keyup.enter="saveChange(todo)">
            <button v-if="showEditBox" @click="hideEditBox" class="btn btn-secondary">cancel</button>
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
    editTodo(){
      this.showEditBox=true;
      console.log(this.showEditBox);
      console.log('editTodo: this.todo: ',this.todo)
    },
    async saveChange(todo){
      console.log('saveChange: this.todo: ', this.todo)
      const res = await axios.put(baseURL+'/'+this.todo.id, { name: this.todoName});
      this.todos = [...this.todos, res.data];
      // this.todoName = '';
      this.showEditBox=false;
      console.log('showEditBox: ', this.showEditBox);
      console.log('edited: ', todo);
      window.location.reload();
    }, 
    hideEditBox(){
      this.showEditBox=true;
      window.location.reload();
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

@import'~bootstrap/dist/css/bootstrap.css'

</style>
