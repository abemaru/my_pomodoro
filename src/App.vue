<script setup lang="ts">
import { storeToRefs } from 'pinia';
import { reactive } from 'vue';
import { useTodoStore } from './store/todos';

const state = reactive({ newTodoLabel: "" });
const store = useTodoStore();
const { filteredTodos, filter } = storeToRefs(store);

const toggleTodo = (id: number) => store.toggleTodo(id);
const addTodo = () => {
  if (state.newTodoLabel !== "") {
    store.addTodo(state.newTodoLabel);
    state.newTodoLabel = "";
  }
};

</script>

<template>
  <input v-model="state.newTodoLabel" />
  <button @click="addTodo">add</button>
  
  <input id="all" type="radio" v-model="filter" value="all" />
  <label for="all">すべて</label>
  <input id="finished" type="radio" v-model="filter" value="finished" />
  <label for="finished">完了済み</label>
  <input id="unfinished" type="radio" v-model="filter" value="unfinished" />
  <label for="unfinished">未完了</label>

  <ul>
    <li
      :class="{ todo: true, finished: todo.finished}"
      :key="todo.label"
      v-for="todo in filteredTodos"
      v-text="todo.label"
      @click="toggleTodo(todo.id)"
    />
  </ul>
</template>

<style scoped>
.todo {
  user-select: none;
  cursor: pointer;
}

.todo.finished {
  text-decoration: line-through;
  color: gray;
}
</style>
