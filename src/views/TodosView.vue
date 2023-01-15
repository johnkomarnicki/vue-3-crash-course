<script setup>
import { uid } from "uid";
import { ref, computed } from "vue";
import TodoCreator from "../components/TodoCreator.vue";
import TodoList from "../components/TodoList.vue";

const todoList = ref([]);

const todosCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

// Fetch Todo's on page load
fetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: null,
  });
  setTodoListLocalStorage();
};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
  setTodoListLocalStorage();
};

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
  setTodoListLocalStorage();
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
  setTodoListLocalStorage();
};

const deleteTodo = (todo) => {
  todoList.value = todoList.value.filter(
    (todoFilter) => todoFilter.id !== todo.id
  );
  setTodoListLocalStorage();
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo">
      <template #button-content>Create</template>
    </TodoCreator>
    <TodoList
      :todoList="todoList"
      :todosCompleted="todosCompleted"
      @edit-todo="toggleEditTodo"
      @update-todo="updateTodo"
      @toggle-complete="toggleTodoComplete"
      @delete-todo="deleteTodo"
    />
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>
