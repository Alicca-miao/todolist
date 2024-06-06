<template>
  <div>
    <h1>ToDo App</h1>
    <form @submit.prevent="addtodo">
      <label>New ToDo </label>
      <input v-model="todo" v-focus>
      <button>todo submit</button>
    </form>

    <h2>ToDo List</h2>
    <ul>
      <li v-for="(todo, index) in todos" :key="index" v-hover>

        <span v-if="!todo.editing" :class="{ done: todo.done }" @click="turntodone(todo)" @dblclick="edittodo(todo)">{{
      todo.content }}</span>

        <input v-else v-model="todo.content" @blur="saveEditedtodo(todo)" @keyup.enter="saveeditedtodo(todo)">

        <button @click="deletetodo(index)">Remove</button>

      </li>
    </ul>
    <div v-if="todos.length===0">Empty</div>

  </div>

</template>

<script setup>
import { ref } from 'vue'

const focusdirective = {

}
let todo = ref('')
let defaultdata = [{
  done: false,
  content: 'what',
  editing: false

}]
const tododata = JSON.parse(localStorage.getItem('todos')) || defaultdata
const todos = ref(tododata)
function addtodo() {
  if (todo.value) {
    let x = {
      done: false,
      content: todo.value
    }
    todos.value.push(x)
    //push时候push的是对象哈，不小心加了个[]
    todo.value = ''
  }
  savedata()
}
function turntodone(todo) {
  todo.done = !todo.done
  savedata()

}

function edittodo(todo) {
  todo.editing = true
}
function saveeditedtodo(todo) {
  todo.editing = false;
  savedata()
}

function savedata() {
  let x = JSON.stringify(todos.value)
  localStorage.setItem('todos', x)
}
function deletetodo(index) {
  todos.value.splice(index, 1)
  savedata()
}




const vFocus = {
  mounted(el) {
    el.focus();
  }
};

const vHover = {
  mounted(el) {
    el.addEventListener('mouseover', handleMouseOver);
    el.addEventListener('mouseout', handleMouseOut);
  },
  unmounted(el) {
    el.removeEventListener('mouseover', handleMouseOver);
    el.removeEventListener('mouseout', handleMouseOut);
  }
};

function handleMouseOver(event) {
  event.target.style.backgroundColor = 'rgba(211, 211, 211, 0.1)';
}

function handleMouseOut(event) {
  // event.target.style.backgroundColor = 'transparent';
}






</script>

<!-- <style scoped>
.li {
    text-decoration: none;
}
.done {
    text-decoration-line: line-through;
}
</style> -->

<style>
/* Variables */
:root {
  --border: 2px solid rgba(4, 4, 4, 0.35);
  --size1: 6px;
  --size2: 12px;
  --size3: 18px;
  --size4: 24px;
  --size5: 48px;
  --backgroundColor: #ced1d9;
  --textColor: rgb(12, 12, 12);
  --primaryColor: #a6f5e2;
  --secondTextColor: #1f2023;
}

/* Global Styles */
body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: var(--backgroundColor);
  color: var(--textColor);
}

#app {
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  padding: 20px;
}

input {
  background-color: transparent;
  border: var(--border);
  color: inherit;
  outline: none;
  /* 添加这行去除蓝色框 */
}

h1 {
  font-weight: bold;
  font-size: 28px;
  text-align: center;
}

form {
  display: flex;
  flex-direction: column;
  width: 100%;
}

label {
  font-size: 14px;
  font-weight: bold;
}

input,
button {
  height: var(--size5);
  box-shadow: none;
  outline: none;
  padding-left: var(--size2);
  padding-right: var(--size2);
  border-radius: var(--size1);
  font-size: 18px;
  margin-top: var(--size1);
  margin-bottom: var(--size2);
}

input {
  background-color: transparent;
  border: var(--border);
  color: inherit;
}

button {
  cursor: pointer;
  background-color: var(--primaryColor);
  border: 1px solid var(--primaryColor);
  color: var(--secondTextColor);
  font-weight: bold;
  border-radius: var(--size1);
}

h2 {
  font-size: 22px;
  border-bottom: var(--border);
  padding-bottom: var(--size1);
}

ul {
  padding: 10px;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: var(--border);
  padding: var(--size2) var(--size4);
  border-radius: var(--size1);
  margin-bottom: var(--size2);
}

span {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}

button {
  font-size: var(--size2);
  padding: var(--size1);
}

h4 {
  text-align: center;
  opacity: 0.5;
  margin: 0;
}
</style>
