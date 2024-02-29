<script setup>
import {onMounted, ref, watch} from 'vue';

const toDos = ref([]);
const text = ref("");

function deleteTodo(todo){
  toDos.value = toDos.value.filter((x) => x !==todo)
}
function addTodo() {
  if (text.value.trim() === "") {
    return;
  }

  toDos.value.unshift({
    todo: text.value,
    done: false,
  });

  text.value = "";
}
watch(toDos,(newToDoValue)=>{
  localStorage.setItem('todos',JSON.stringify(newToDoValue));
},
{
  deep:true
}
);

onMounted(()=>{
  toDos.value = JSON.parse(localStorage.getItem('todos')) || [];
})
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">✍️ToDo Application</h3>
    </section>

    <div class="input-section">
      <section class="create-todo">
        <form @submit.prevent="addTodo">
          <h3>What do you plan on doing🙂?</h3>
          <input
            type="text"
            placeholder="e.g. email your boss"
            v-model="text"
          />

          <input type="submit" value="Add todo" />
        </form>
      </section>
    </div>

    <div class="todo-section">
      <section class="todo-list">
        <h2 v-show="toDos.length === 0">No Todos Here😞</h2>

        <div class="list">
          <div
            v-for="todo in toDos"
            :class="`todo-item ${todo.done && 'done'}`"
          >
            <label>
              <input type="checkbox" v-model="todo.done" />
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.todo" />
            </div>

            <div class="actions">
              <button class="delete" @click="deleteTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>