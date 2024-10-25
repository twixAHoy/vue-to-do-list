This "Todo List" project was created to help understand Vue.js better and to address some of the basics of Vue. This project does not currently connect to a database and uses local storage.

This project uses tailwind.css for styling(https://v2.tailwindcss.com/docs/guides/vue-3-vite)

Notes:

1. Within App.vue we are importing the following from the vue library for the following reasons:

   - onmounted: Used to get data from local storage.
   - computed: Used to order dates by the date and time they were created.
   - watch: Watches for any changes in our references and updates local storage accordingly.

2. Using the 'deep' option within the watch function tells Vue to look through the array that it's watching and compare it to what's stored in local storage. If anything has changed, the watch function will run again.

3. This project does not include components yet.. the purpose of this project was to understand how to bring up a Vue project and see "real time" changes.
