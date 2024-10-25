<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");
const input_content = ref("");
const input_category = ref(null);

const todo_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_content.value = "";
  input_category.value = null;
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title text-3xl text-black mt-10 mb-12 ml-24">
        What's up,
        <input
          class="text-purple-400"
          type="text"
          placeholder="Name here"
          v-model="name"
        />
      </h2>
    </section>

    <div class="flex p-8 columns-2 gap-16 ml-24">
      <section class="create-todo mb-24">
        <form @submit.prevent="addTodo" class="">
          <h4 class="mb-1">What's on your todo list?</h4>
          <input
            class="border-2 border-slate-200 rounded-md w-80 mb-4 p-2"
            type="text"
            placeholder="ex: make a video"
            v-model="input_content"
          />
          <h4 class="mb-1">Pick a Category</h4>
          <div class="options flex grow mb-8">
            <label class="mr-24">
              <input
                type="radio"
                name="category"
                id="category_1"
                value="business"
                v-model="input_category"
              />
              <span class="bubble business"></span>
              <div>Business</div>
            </label>
            <label>
              <input
                type="radio"
                name="category"
                id="category_2"
                value="personal"
                v-model="input_category"
              />
              <span class="bubble personal"></span>
              <div>Personal</div>
            </label>
          </div>

          <div class="flex flex-col">
            <input
              class="bg-pink-500 w-80 p-4 text-center text-white rounded-md hover:bg-pink-300 cursor-grab mb-4"
              type="submit"
              value="Add todo"
            />
            <input
              class="bg-green-500 w-80 p-4 text-center text-white rounded-md hover:bg-green-300 cursor-grab"
              type="submit"
              value="Add new category"
            />
          </div>
        </form>
      </section>

      <div class="flex">
        <section class="todo-list-personal flex flex-col">
          <h3 class="mb-4">PERSONAL</h3>
          <div class="list">
            <div
              v-for="(todo, index) in todo_asc.filter(
                (todo) => todo.category === 'personal'
              )"
              :key="index"
              :class="`todo-item flex justify-center bg-yellow-200 m-2 p-2 h-44 w-72 shadow-lg shadow-slate-300 rounded-sm
            bg-gradient-to-b from-yellow-300 to-yellow-200
            ${todo.done && 'done'}`"
            >
              <label>
                <input type="checkbox" class="text-xl" v-model="todo.done" />
                <span :class="`bubble mr-2 ${todo.category}`"></span>
              </label>

              <div class="todo-content">
                <textarea
                  type="text"
                  class="text-3xl bg-transparent amatic-sc-regular text-wrap max-h-min max-w-min resize-none"
                  :class="{ 'line-through': todo.done }"
                  v-model="todo.content"
                ></textarea>
              </div>

              <div class="actions">
                <button
                  class="delete bg-red-600 text-white p-2 w-16 rounded-md bottom-0 right-0 hover:bg-red-500"
                  @click="removeTodo(todo)"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </section>
        <section class="todo-list-business flex flex-col">
          <h3 class="mb-4">BUSINESS</h3>
          <div class="list">
            <div
              v-for="(todo, index) in todo_asc.filter(
                (todo) => todo.category === 'business'
              )"
              :key="index"
              :class="`todo-item flex justify-center bg-yellow-200 m-2 p-2 h-44 w-72 shadow-lg shadow-slate-300 rounded-sm
            bg-gradient-to-b from-purple-300 to-purple-200
            ${todo.done && 'done'}`"
            >
              <label>
                <input type="checkbox" class="text-xl" v-model="todo.done" />
                <span :class="`bubble mr-2 ${todo.category}`"></span>
              </label>

              <div class="todo-content">
                <textarea
                  type="text"
                  class="text-3xl bg-transparent max-h-min max-w-min amatic-sc-regular resize-none"
                  :class="{ 'line-through': todo.done }"
                  v-model="todo.content"
                ></textarea>
              </div>

              <div class="actions">
                <button
                  class="delete bg-red-600 text-white p-2 w-16 rounded-md bottom-0 right-0 hover:bg-red-500"
                  @click="removeTodo(todo)"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </main>
</template>
