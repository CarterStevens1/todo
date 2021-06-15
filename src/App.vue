<template>
  <h1>ToDo App</h1>
  <!-- prevents the form from being submitted after adding a todo -->
  <form @submit.prevent="addTodo()">
    <label>New ToDo </label>
    <input v-model="newTodo" name="newTodo" autocomplete="off" />
    <label>Description</label>
    <input v-model="todoDesc" name="todoDesc" autocomplete="off" />
    <button>Add ToDo</button>
  </form>
  <h2>ToDo List</h2>
  <ul>
    <!-- iterates through the todo in todos to print out each item. -->
    <li v-for="(todo, index) in todos" :key="index">
      <span :class="{ done: todo.done }" @click="doneTodo(todo)"
        >{{ todo.content }} <br />

        <p class="description">{{ todo.desc }}</p></span
      >
      <!-- @click to remove the todo item -->
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
  <!-- displays if no items in the todolist -->
  <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    const newTodo = ref("");
    const todoDesc = ref("");
    const defaultData = [
      {
        done: false,
        content: "Write a blog post",
      },
    ];
    //gets the item from the localstorage if there is any and returns it back into the site if it has storage. if not it prints the default data
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);
    //pushes a new item into the list
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
          desc: todoDesc.value,
        });

        newTodo.value = "";
        todoDesc.value = "";
      }
      saveData();
    }
    //if the todo is not done change it to done
    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }
    //saves the data for the site /localhost
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }
    //returns each function for use
    return {
      todos,
      newTodo,
      todoDesc,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
};
</script>

<style lang="scss">
$border: 2px solid
  rgba(
    $color: white,
    $alpha: 0.35,
  );
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #27292d;
$textColor: white;
$primaryColor: #013b12;
$secondTextColor: #ffffff;
body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: $backgroundColor;
  color: $textColor;
  #app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    h1 {
      font-weight: bold;
      font-size: 2em;
      text-align: center;
    }
    form {
      display: flex;
      flex-direction: column;
      width: 100%;
      label {
        font-size: 14px;
        font-weight: bold;
      }
      input,
      button {
        height: $size5;
        box-shadow: none;
        outline: none;
        padding-left: $size2;
        padding-right: $size2;
        border-radius: $size1;
        font-size: 18px;
        margin-top: $size1;
        margin-bottom: $size2;
      }
      input {
        background-color: transparent;
        border: $border;
        color: inherit;
      }
    }
    button {
      cursor: pointer;
      background-color: $primaryColor;
      border: 1px solid $primaryColor;
      color: $secondTextColor;
      font-weight: bold;
      outline: none;
      border-radius: $size1;
    }
    h2 {
      font-size: 22px;
      border-bottom: $border;
      padding-bottom: $size1;
    }
    ul {
      padding: 10px;
      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: $border;
        padding: $size2 $size4;
        border-radius: $size1;
        margin-bottom: $size2;
        span {
          cursor: pointer;
        }
        .description {
          font-size: 20px;
        }
        .done {
          text-decoration: line-through;
          color: $primaryColor;
        }
        button {
          font-size: $size2;
          padding: $size1;
        }
      }
    }
    h4 {
      text-align: center;
      opacity: 0.5;
      margin: 0;
    }
  }
}
</style>
