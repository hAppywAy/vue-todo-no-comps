<template>
  <div id="app" class="min-h-screen h-full" style="background: #edf2f7;">
    <div class="flex items-center justify-center">
      <div class="w-full flex items-center justify-center font-sans">
        <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg overflow-y-auto">
          <div class="mb-4">
              <h1 class="text-3xl text-gray-900">Ma Todo</h1>
              <div class="flex mt-4">
                  <input
                      v-model="description"
                      class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-gray-700"
                      placeholder="Add Todo"
                      @keyup.enter="addTodo()"
                  >
                  <button
                      @click="addTodo()"
                      class="flex p-2 border-2 rounded text-blue-500 border-blue-500 hover:text-white hover:bg-blue-500"
                  >Ajouter</button>
              </div>
          </div>
          <div class="mb-5">
            <div class="mb-2">
                {{ progressPct }}% réalisé
            </div>
            <div class="w-full bg-gray-200 rounded-full h-2.5 dark:bg-gray-700">
                <div class="bg-blue-600 h-2.5 rounded-full" :style="`width: ${progressPct}%`"></div>
            </div>
          </div>
          <div>
              <div class="flex mb-4 items-center" v-for="todo in paginatedTodos" :key="todo.id">
                  <p class="w-full text-gray-900" :class="{
                    'line-through': todo.done
                  }">{{ todo.description }}</p>
                  <button
                    @click="checkTodo(todo)"
                    class="flex p-2 ml-4 mr-2 border-2 rounded hover:text-white text-green-500 border-green-500 hover:bg-green-500"
                    :class="{
                      'text-gray-500 border-gray-500 hover:bg-gray-500': todo.done,
                      'text-green-500 border-green-500 hover:bg-green-500': !todo.done
                    }"
                  >{{ todo.done ? "Fait" : "Pas fait" }}</button>
                  <button
                      @click="deleteTodo(todo.id)"
                      class="flex p-2 ml-2 border-2 rounded text-red-500 border-red-500 hover:text-white hover:bg-red-500"
                  >Supprimer</button>
              </div>
          </div>
          <div class="flex justify-center mt-10">
            <nav class="relative z-0 inline-flex rounded-md shadow-sm -space-x-px" aria-label="Pagination">

              <a href="#"
                 v-if="page !== 1"
                 class="relative inline-flex items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                <span class="sr-only">Previous</span>
                <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
                </svg>
              </a>
              <a
                  v-for="currentPage in availablePages" :key="currentPage"
                  href="#"
                  class="bg-white border-gray-300 text-gray-500 hover:bg-gray-50 relative inline-flex items-center px-4 py-2 border text-sm font-medium"
                  @click="setPage(currentPage)"
              > {{currentPage}} </a>
              <a
                  v-if="!isCurrentPageNotLastPage"
                  href="#" class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                <span class="sr-only">Next</span>
                <!-- Heroicon name: solid/chevron-right -->
                <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                </svg>
              </a>
            </nav>
          </div>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
const PAGE_SIZE = 5;

export default {
  name: 'App',
  data() {
    return {
      page: 1,
      description: "",
      todos: [
        {
          id: 1,
          done: false,
          description: 'Ajouter une todo avec Vue 2',
        },
        {
          id: 2,
          done: false,
          description: 'Design de ma Todo',
        },
      ]
    }
  },
  computed: {
    idMax() {
      return Math.max(...this.todos.map(todo => todo.id)) + 1;
    },
    progressPct() {
      return Math.round((this.todos.filter(todo => todo.done).length / this.todos.length) * 100);
    },
    paginatedTodos() {
      // slice: startIndex, endIndex
      // splice: startIndex, longueur a partir de startIndex a supprimer, element(s) qui remplacent
      return this.todos.slice((this.page - 1) * PAGE_SIZE, this.page * PAGE_SIZE);
    },
    isCurrentPageNotLastPage() {
      return this.page >= (this.todos.length / PAGE_SIZE);
    },
    availablePages() {
      const pages = [];
      const max = (Math.floor(this.todos.length / PAGE_SIZE) + 1);
      console.log(this.page);
      console.log(max);
      if (this.page - 3 > 0) pages.unshift(this.page - 3);
      if (this.page - 2 > 0) pages.unshift(this.page - 2);
      if (this.page - 1 > 0) pages.unshift(this.page - 1);
      pages.push(this.page);
      if (this.page + 1 <= max) pages.push(this.page + 1);
      if (this.page + 2 <= max) pages.push(this.page + 2);
      if (this.page + 3 <= max) pages.push(this.page + 3);
      return pages;
    },
  },
  methods: {
    setPage(page) {
      this.page = page;
    },
    checkTodo(todo) {
      todo.done = !todo.done;
    },
    deleteTodo(todoId) {
      const todoIndexToDelete = this.todos.findIndex(todo => todo.id === todoId);
      this.todos.splice(todoIndexToDelete, 1);
    },
    addTodo() {
      // const newTodos = [...this.todos];
      // this.todos = newTodos;
      if (this.description) {
        this.todos.unshift({
          description: this.description,
          id: this.idMax,
          done: false
        });
        this.description = "";
      }
    }
  }
}
</script>
