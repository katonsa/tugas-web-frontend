<template>
  <div>
    <h1>Daftar Tugas</h1>
    <ul>
      <li v-for="item in todos" :key="item.id">{{ item.description }}<button @click="hapus(item.id)">X</button></li>
    </ul>
    <input v-model="myText"/>
    <button @click="tambah">Add</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: function() {
    return {
      todos: [],
      myText: '',
      connection: null,
    }
  },
  created: function () {
    const username = localStorage.getItem('usr')
    const password = localStorage.getItem('pwd')
    axios.get('http://localhost:3000/todo', { headers: {username, password}})
      .then(result=>{
        this.todos = result.data
      })
    
    console.log("Starting connection to WebSocket Server")
    this.connection = new WebSocket("ws://localhost:3000")
    this.connection.onmessage = (event) => {
      const deserialized = JSON.parse(event.data)
      switch (deserialized.action) {
        case 'ADD_ITEM':
          this.todos.push(deserialized.data);
          break;
        case 'REMOVE_ITEM':
            for(var i = 0; i < this.todos.length; i++){
              if(this.todos[i].id == deserialized.data.id) {
                this.todos.splice(i, 1)
              }
            }
          break;
        default:
          break;
      }
    }
    this.connection.onopen = function(event) {
      console.log("Successfully connected to the echo websocket server...")
    }
  },
  methods: {
    tambah: function (){
      const newItem = {description: this.myText}
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.post('http://localhost:3000/todo', newItem, { headers: {username, password}})
        .then(() => {});
      
      this.myText = '';
    },
    hapus: function (id) {
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.delete(`http://localhost:3000/todo/${id}`, { headers: {username, password}})
        .then(()=>{})
    }
  }
}
</script>

