<template>
  <div class="home">
    <h1>Options API Method</h1>
    <h2>Available todo is {{ todoCount }}</h2>
    <h4>Completed todo is {{completeCount}}</h4>
    <h4>Uncompleted todo is {{uncompleteCount}}</h4>
    <input
      type="text"
      placeholder="Add todo"
      v-model="addedTodo"
      @keydown.enter="addNewTodo"
    />
    <ul>
      <li v-for="(todo, index) in todos" :key="todo.id">
        <span :class="todo.complete ? 'check' : 'uncheck' " >{{ todo.todoName }}</span>
        <button @click="deleteTodo(index)"  >X</button>
        <button @click="check(todo)">✅</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { toRef } from '@vue/reactivity';
// @ is an alias to /src
export default {
  data() {
    return {
      addedTodo: "",
      todos: [
        {
          id: 1,
          todoName: "One",
          complete: true
        },
        {
          id: 2,
          todoName: "Two",
          complete: false
        },
        {
          id: 3,
          todoName: "Three",
          complete: false
        },
      ],
      isCompleted: true,
      activeId: 0,
      bannedWords: ["fuck", "bitch", "nigga", "bitcoin"]
    };
  },
  computed: {
    todoCount() {
      return this.todos.length;
    },
    completeCount(){
      let completedTodo = this.todos.filter(todo => {
        return todo.complete === true
      })
      return completedTodo.length
    },
    uncompleteCount(){
      let uncompletedTodo = this.todos.filter(todo => {
        return todo.complete === false
      })
      return uncompletedTodo.length
    }
  },
  watch:{
    addedTodo(newValue){  
      console.log("newValue: ", newValue)
      if(this.bannedWords.includes(newValue.toLowerCase())){
        this.addedTodo = ""
        console.log(`You can never use ${newValue} `)
      }
    }
  },
  methods: {
    addNewTodo() {
      // let { addedTodo, todos }  = this
      let newTodo = {
        id: Date.now(),
        todoName: this.addedTodo,
        complete: false 
      };
      this.todos.push(newTodo);
      this.addedTodo = "";
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    check(todo){
      todo.complete = !todo.complete
    }
  },
};
</script>

<style>
ul {
  list-style: none;
  padding: 0;
  width: 200px;
  margin: 20px auto 0;
}

li {
  border: 1px solid #e2e2e2;
  display: flex;
  margin-bottom: 20px;
}

li span {
  flex-grow: 1;
}

.check{
  text-decoration: line-through;
  opacity: .5;
}

.uncheck{
  text-decoration: none;
}
</style>
