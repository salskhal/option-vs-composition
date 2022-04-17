<template>
  <div class="home">
    <h1>Composition API Method</h1>
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
        <button @click="deleteTodo(index)">X</button>
        <button @click="check(todo)">✅</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
import { computed, watch } from '@vue/runtime-core';
// @ is an alias to /src
export default {
  setup() {
    const todos = ref([
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
    ]);
    const addedTodo = ref("");
    
    const bannedWords = ["fuck", "bitch", "nigga", "bitcoin"]

    const addNewTodo = () => {
      let newTodo = {
        id: Date.now(),
        todoName: addedTodo.value,
        complete: false
      };
    
      todos.value.push(newTodo)
      addedTodo.value = ""
    };

    const deleteTodo = (index) => {
      todos.value.splice
      (index, 1)
    }

    const todoCount = computed(() =>{
      return todos.value.length
    })

    const completeCount = computed(() =>{
      let completedTodo = todos.value.filter(todo => {
        return todo.complete === true
      })
      return completedTodo.length
    })

    const uncompleteCount = computed(() =>{
       let uncompletedTodo = todos.value.filter(todo => {
        return todo.complete === false
      })
      return uncompletedTodo.length
    })

    const check = (todo) => {
      todo.complete = !todo.complete
    } 


    watch(addedTodo, (newValue) => {
      console.log("newValue: ", newValue)
      if(bannedWords.includes(newValue.toLowerCase())){
        addedTodo.value = ""
        alert(`You can never use ${newValue} `)
      }
    })

    return { todos, addedTodo, addNewTodo, deleteTodo, todoCount, check,  completeCount, uncompleteCount};
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

.check {
  text-decoration: line-through;
}
</style>
