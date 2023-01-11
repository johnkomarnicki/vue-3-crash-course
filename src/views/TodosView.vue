<script setup>
import { uid } from "uid";
import { ref } from "vue";
import TodoCreator from "../components/TodoCreator.vue";

const todoList = ref([]);
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: null,
  });
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <CreateTodo @create-todo="createTodo">
      <template #button-content>Create</template>
    </CreateTodo>
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
