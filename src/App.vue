<template class="">
  <!-- Main Container -->
  <div
    class="flex flex-col items-center justify-center mx-auto mt-10 bg-white"
  >
    <h1 class="text-2xl text-center font-semibold">Todo List</h1>

    <!-- Input and Add Btn -->
    <div class="flex flex-col mx-auto mt-2 space-y-2">
      <input
        type="text"
        v-model="newTodo"
        @keyup.enter="addTodo"
        class="mx-auto p-1 rounded-md border-b-gray-500 border-2 active:border-none focus:outline-none"
        placeholder="..."
      />
      <button @click="addTodo" class="rounded-full opacity-90 hover:opacity-100 transition-opacity duration-700 bg-green-500 text-white mx-auto py-1 px-3">
        Add
      </button>
    </div>

    <!-- Show -->
    <div class="flex flex-col items-center mt-16 min-w-full">
      <h3 class="py-4 text-xl font-medium">Items:</h3>

      <ul class="flex flex-col items-start space-y-4 ">
        <li v-for="(todo, index) in todos" :key="index" class="relative min-w-full">
          <!-- Item Container -->
          <div class="flex space-x-4 justify-between relative min-w-full">
            <input type="checkbox" v-model="todo.completed" />
            <span :class="{ completed: todo.completed }" class="w-1/2">{{ todo.text }}</span>
            <button
              @click="removeTodo(index)"
              class="rounded-full bg-red-500 opacity-80 hover:opacity-100 transition-opacity duration-500 text-white py-1 px-3 ml-10 tracking-tight"
            >
              Delete
            </button>
          </div>
        </li>
      </ul>
    </div>



  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: []
    }
  },
  mounted() {
    if (localStorage.getItem('todos')) {
      try {
        this.todos = JSON.parse(localStorage.getItem('todos'))
      } catch (e) {
        localStorage.removeItem('todos')
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo, completed: false })
        this.newTodo = ''
        this.saveToLocalStorage()
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }
}
</script>
