<script setup>
import { Icon } from "@iconify/vue";
const props = defineProps({
  todoList: {
    type: Array,
    default: () => [],
  },
  todosCompleted: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits([
  "toggle-complete",
  "update-todo",
  "edit-todo",
  "delete-todo",
]);
</script>

<template>
  <ul class="todo-list" v-if="todoList.length > 0">
    <li v-for="(todo, index) in todoList">
      <input
        type="checkbox"
        :value="todo.isCompleted"
        :checked="todo.isCompleted"
        @input="$emit('toggle-complete', index)"
      />
      <div class="todo">
        <input
          v-if="todo.isEditing"
          type="text"
          :value="todo.todo"
          @input="$emit('update-todo', $event.target.value, index)"
        />
        <span
          v-else
          :class="{
            'completed-todo': todo.isCompleted,
          }"
        >
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
          @click="$emit('edit-todo', index)"
        />
        <Icon
          v-else
          icon="ph:pencil-fill"
          class="icon edit-icon"
          color="41b080"
          width="22"
          @click="$emit('edit-todo', index)"
        />
        <Icon
          icon="ph:trash"
          class="icon trash-icon"
          color="f95e5e"
          width="22"
          @click="$emit('delete-todo', todo)"
        />
      </div>
    </li>
  </ul>
  <p v-else class="todos-msg">
    <Icon icon="noto-v1:sad-but-relieved-face" />
    <span>You have no todo's to complete! Add one!</span>
  </p>
  <p v-if="todosCompleted && todoList.length > 0" class="todos-msg">
    <Icon icon="noto-v1:party-popper" />
    <span>You have completed all your todos!</span>
  </p>
</template>

<style lang="scss" scoped>
.todo-list {
  display: flex;
  flex-direction: column;
  list-style: none;
  margin-top: 24px;
  gap: 20px;

  li {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 16px 10px;
    background-color: #f1f1f1;
    box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
      0 8px 10px -6px rgb(0 0 0 / 0.1);

    &:hover {
      .todo-actions {
        opacity: 1;
      }
    }

    input[type="checkbox"] {
      appearance: none;
      width: 20px;
      height: 20px;
      background-color: #fff;
      border-radius: 50%;
      box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1),
        0 2px 4px -2px rgb(0 0 0 / 0.1);

      &:checked {
        background-color: #41b080;
      }
    }

    .todo {
      flex: 1;

      .completed-todo {
        text-decoration: line-through;
      }

      input[type="text"] {
        width: 100%;
        padding: 2px 6px;
        border: 2px solid #41b080;
      }
    }

    .todo-actions {
      display: flex;
      gap: 6px;
      opacity: 0;
      transition: 150ms ease-in-out;
      .icon {
        cursor: pointer;
      }
    }
  }
}

.todos-msg {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 24px;
}
</style>
