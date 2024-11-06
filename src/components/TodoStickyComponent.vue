<script>
//define props
export default {
  props: {
    typeOfTodo: {
      type: String,
      required: true,
    },
    todo_asc: {
      type: Array,
      required: true,
    },
    todo_color: {
      type: String,
      required: true,
    },
  },
};
</script>

<template>
  <section class="todo-list-business flex flex-col">
    <h3 class="mb-4">{{ typeOfTodo }}</h3>
    <div class="list">
      <div
        v-for="(todo, index) in todo_asc.filter(
          (todo) => todo.category === typeOfTodo
        )"
        :key="index"
        :class="`todo-item flex justify-center ${todo_color} m-2 p-2 h-44 w-72 shadow-lg shadow-slate-300 rounded-sm
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
            @click="$emit('removeTodo', todo)"
          >
            Delete
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
