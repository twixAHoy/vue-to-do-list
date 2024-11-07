<script setup>
import { ref, onMounted, computed, watch } from "vue";
import TodoStickyComponent from "./components/TodoStickyComponent.vue";
import AddButtonComponent from "./components/AddButtonComponent.vue";

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
            <AddButtonComponent
              :backgroundColor="'bg-pink-500 w-80'"
              :hoverColor="'hover:bg-pink-300'"
              :buttonValue="'Add todo'"
            />
            <AddButtonComponent :buttonValue="'Add new category'" />
          </div>
        </form>
      </section>

      <div class="flex">
        <TodoStickyComponent
          :typeOfTodo="'personal'"
          :todo_asc="todo_asc"
          :todo_color="'bg-yellow-200 bg-gradient-to-b from-yellow-300 to-yellow-200'"
          @removeTodo="removeTodo"
        />
        <TodoStickyComponent
          :typeOfTodo="'business'"
          :todo_asc="todo_asc"
          :todo_color="'bg-purple-200 bg-gradient-to-b from-purple-300 to-purple-200'"
          @removeTodo="removeTodo"
        />
      </div>
    </div>
  </main>
</template>
