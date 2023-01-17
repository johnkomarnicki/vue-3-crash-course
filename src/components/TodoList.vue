<script setup>
import { Icon } from "@iconify/vue";
const props = defineProps({
  todoList: {
    type: Array,
    default: () => [],
  },
});
</script>

<template>
  <ul class="todo-list" v-if="todoList.length > 0">
    <li v-for="todo in todoList">
      <input type="checkbox" :value="todo.isCompleted" />
      <div class="todo">
        <input v-if="todo.isEditing" type="text" :value="todo.todo" />
        <span v-else>
          {{ todo.todo }}
        </span>
      </div>
      <div class="todo-actions">
        <Icon
          v-if="todo.isEditing"
          icon="ph:check-circle"
          class="icon check-icon"
          color="41b080"
          width="22"
        />
        <Icon
          v-else
          icon="ph:pencil-fill"
          class="icon edit-icon"
          color="41b080"
          width="22"
        />
        <Icon
          icon="ph:trash"
          class="icon trash-icon"
          color="f95e5e"
          width="22"
        />
      </div>
    </li>
  </ul>
  <p v-else class="todos-msg">
    <Icon icon="noto-v1:sad-but-relieved-face" />
    <span>You have no todo's to complete! Add one!</span>
  </p>
</template>

<style lang="scss" scoped></style>
