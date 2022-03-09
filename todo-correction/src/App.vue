<template>
  <div id="app" class="min-h-screen h-full" style="background: #edf2f7;">
    <div class="flex items-center justify-center">
      <div class="w-full flex items-center justify-center font-sans">
        <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg overflow-y-auto">
              <div class="mb-4">
                  <h1 class="text-3xl text-gray-900">Ma Todo</h1>
                  <div class="flex mt-4">
                      <input v-model="description" class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-gray-700" placeholder="Add Todo">
                      <button @click="addTodo()" class="flex p-2 border-2 rounded text-blue-500 border-blue-500 hover:text-white hover:bg-blue-500">Ajouter</button>
                  </div>
              </div>
              <div class="mb-5">
                <div class="mb-2">
                    {{percentageDone}} réalisé
                </div>
                <div class="w-full bg-gray-200 rounded-full h-2.5 dark:bg-gray-700">
                    <div class="bg-blue-600 h-2.5 rounded-full" :style="{width: percentageDone}"></div>
                </div>
              </div>
              <div>
                  <div v-for="todo in todos" :key="todo.id" class="flex mb-4 items-center">
                      <p class="w-full text-gray-900" :class="{
                        'line-through': todo.done
                      }">{{todo.description}}</p>
                      <button @click="toggleDone(todo.id)" v-if="!todo.done" class="flex p-2 ml-4 mr-2 border-2 rounded hover:text-white text-green-500 border-green-500 hover:bg-green-500">Fait</button>
                      <button @click="toggleDone(todo.id)" v-else-if="todo.done" class="flex p-2 ml-4 mr-2 border-2 rounded hover:text-white text-gray-500 border-gray-500 hover:bg-gray-500">Pas fait</button>
                      <button @click="deleteTodo(todo.id)" class="flex p-2 ml-2 border-2 rounded text-red-500 border-red-500 hover:text-white hover:bg-red-500">Supprimer</button>
                  </div>
              </div>
          </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      description: "",
      todos: [
        {
          id: 1,
          done: false,
          description: 'Ajouter une todo avec Vue',
        },
        {
          id: 2,
          done: true,
          description: 'Design de ma Todo',
        },
      ]
    }
  },
  computed: {
    percentageDone () {
      return `${this.todos.length > 0 ? (((this.todos.filter(todo => todo.done)).length / this.todos.length) * 100).toFixed(2) : 0}%`;
    },
    maxTodoId () {
      return Math.max(...this.todos.map(todo => todo.id));
    }
  },
  methods: {
    deleteTodo(id) {
      this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1);
    },
    addTodo() {
      if (this.description) {
        this.todos.unshift({id: this.maxTodoId + 1, description: this.description, done: false});
        this.description = "";
      }
    },
    toggleDone(id) {
      const todoToEdit = this.todos.find(todo => todo.id === id);
      if (todoToEdit) {
        todoToEdit.done = !todoToEdit.done;
      }
    }
  }
}
</script>
