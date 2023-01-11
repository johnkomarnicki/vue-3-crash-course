<script setup>
import { reactive } from "vue";

const emit = defineEmits(["create-todo"]);

const todo = reactive({
  todo: "",
  invalid: false,
  errMsg: "",
});

const createTodo = () => {
  todo.invalid = false;
  if (todo.todo !== "") {
    emit("create-todo", todo.todo);
    todo.todo = "";
    return;
  }
  todo.invalid = true;
  todo.errMsg = "Todo value cannot be empty!";
};
</script>

<template>
  <div class="input-wrap" :class="{ 'input-err': todo.invalid }">
    <input type="text" v-model="todo.todo" />
    <button @click="createTodo()">
      <slot name="button-content"> Create </slot>
    </button>
  </div>
  <p class="err-msg" v-if="todo.invalid">{{ todo.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  button {
    padding: 8px 16px;
    border: none;
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
