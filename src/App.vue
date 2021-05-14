<template>
  <div id="app">
    <h1><strong>Todos los todos</strong></h1>
    <input type="text" v-model="todoName" :placeholder="'add your todo'" style="margin-bottom: 1em" maxlength="25">
    <button @click="addTodo" class="btn btn-primary">save</button>
      <table>
          <tr>
            <th><h2>Task</h2></th>
            <th><h2>Status</h2></th>
            <th><h2>Actions</h2></th>
          </tr>
          <tr v-for="todo of todos" :key="todo.id">
            <td>
              <span v-if="!showEditBox" class="todoName">{{todo.name}}</span>
              <input v-if="showEditBox" type="text" :value="todo.name" :key="todo.id" maxlength="25">
            </td>
            <td>
              <input type="checkbox" id="checkbox" v-model="checked">
            <label for="checkbox">{{ checked }}</label>
            </td>
            <td>
            <button v-if="!showEditBox" :key="todo.id" @click="editTodo" class="btn btn-primary">edit</button>
            <button v-if="!showEditBox" @click="deleteTodo(todo)" :key="todo.id" class="btn btn-danger">delete</button>
            <button v-if="showEditBox" @click="saveChange(todo)" class="btn btn-primary">save</button>
            <button v-if="showEditBox" @click="hideEditBox" class="btn btn-secondary">cancel</button>
            </td>
          </tr>
      </table>
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
      showEditBox: Boolean,
      completed: Boolean
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
    saveChange(todo){
      console.log('saveChange: this.todo: ', this.todo)
      this.todo.name = this.todoName;
      const res = axios.put(baseURL+'/'+todo.id, { name: this.todoName});
      this.todos = [...this.todos, res.data];
      this.showEditBox=false;
      console.log('showEditBox: ', this.showEditBox);
      console.log('edited: ', todo);
      // window.location.reload();
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
  display:table;
  margin: 0 auto;
}

li {
    list-style: none;
    padding: 1em;
    border-style: solid;
    border-width: 1.5px;
    border-color: grey;
    width: 30em;
    display: flex;
    align-items:baseline;
  }

  td, tr{
    width: 20em;
      /* overflow:hidden; */
  word-wrap:break-word;
  }

table{
    border-style: solid;
    border-width: 1.5px;
    border-color: grey;
    width: 60%;
    margin-left: auto;
    margin-right: auto;
}

span{
  padding-right: 1em;
}

body{
  background-color:rgb(198, 220, 223);
}

span{
  font-size: 1.5em;
}

button{
  display: flex;
  justify-content:center;
  padding: 1em;
  /* position: fixed; */

  }

@import'~bootstrap/dist/css/bootstrap.css'

</style>
