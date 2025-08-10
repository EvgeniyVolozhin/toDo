<script lang="ts">
import { defineComponent } from 'vue';
import AppHeader from './components/appHeader.vue';
import AppFilters from './components/appFilters.vue';
import AppTodoList from './components/appTodoList.vue';
import AppAdTodo from './components/appAdTodo.vue';
import AppFooter, {Stats} from './components/appFooter.vue';
import { Todo } from './types/todo';
import { Filter } from './types/filter'

interface State {
  todos: Todo[],
  activeFilter: 'All' | 'Active' | 'Done'
}


export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppTodoList,
    AppAdTodo,
    AppFooter
  },
  data(): State {
    return {
      todos: [
        { id: 0, text: 'Learn the basics of Vue', completed: true },
        { id: 1, text: 'Learn the basics of Typescript', completed: false },
        { id: 2, text: 'Subscribe to the channel', completed: false },
      ],
      activeFilter: 'All' as 'All' | 'Active' | 'Done',
    }
  },
  computed: {
    filterTodos(): Todo[] {
      switch (this.activeFilter) {
        case 'Active':
          return this.todos.filter(todo => !todo.completed)
        case 'Done':
          return this.todos.filter(todo => todo.completed)
        case 'All':
        default:
          return this.todos
      }
    },
    stats(): Stats {
      return {
        active: this.filterTodos.filter(todo => !todo.completed).length,
        done: this.filterTodos.filter(todo => todo.completed).length,
      }
    }
  },
  methods: {
    addTodo(todo: Todo) {
      this.todos.push(todo)
    },
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)


      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter
    }
  }
})

</script>

<template>
  <AppHeader />
  <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />
  <main class="app-main">
    <AppTodoList :todos="filterTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo" />

    <AppAdTodo @add-todo="addTodo" />

    <AppFooter :stats="stats" />
  </main>


</template>
