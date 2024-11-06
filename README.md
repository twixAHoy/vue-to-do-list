This "Todo List" project was created to help understand Vue.js better and to address some of the basics of Vue. This project does not currently connect to a database and uses local storage.

This project uses tailwind.css for styling(https://v2.tailwindcss.com/docs/guides/vue-3-vite)

Notes:

1. Within App.vue we are importing the following from the vue library for the following reasons:

   - onmounted: Used to get data from local storage.
   - computed: Used to order dates by the date and time they were created.
   - watch: Watches for any changes in our references and updates local storage accordingly.

2. Using the 'deep' option within the watch function tells Vue to look through the array that it's watching and compare it to what's stored in local storage. If anything has changed, the watch function will run again.

3. This project does not include components yet.. the purpose of this project was to understand how to bring up a Vue project and see "real time" changes.

---

Components Notes:

---

1. Make sure you import your child components:
   import TodoStickyComponent from "./components/TodoStickyComponent.vue";
2. When you want to pass properties from a parent component to a child component, you must define your props in the child like so:
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

Then when you call the child component in your parent component, you pass props like this:
<TodoStickyComponent
:typeOfTodo="'personal'"
:todo_asc="todo_asc"
:todo_color="'bg-yellow-200 bg-gradient-to-b from-yellow-300 to-yellow-200'"
@removeTodo="removeTodo"
/>

3. This line of code: @click="$emit('removeTodo', todo), is used to trigger a custom event called removeTodo in Vue when the button is clicked
   - The $emit function is a Vue method that allows a child component to emit (or "send") a custom event to its parent component.
   - In this example, it emits an event called removeTodo and passes the todo object as an argument. This todo object represents the specific item that should be removed from the list
   - In the parent component, this event is listened for using @removeTodo="removeTodo". When the removeTodo event is emitted or triggered by the child, the parent component’s removeTodo method is called, with the todo object as an argument
