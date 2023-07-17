<template>
  <div id="app">
    <pp-header/>
    <pp-filters
        :active-filter="activeFilter"
        @setFilter="setFilter"
    />
    <main class="app-main">
      <pp-todo-list
          :todos="filteredTodos"
          @toggleTodo="toggleTodo"
          @removeTodo="removeTodo"
      />
      <pp-add-todo
          @addTodo="addTodo"
      />
    </main>
    <pp-footer :stats="stats"/>
  </div>
</template>

<script lang="ts">
import {defineComponent} from "vue";
import {Todo} from "@/types/todo";
import PpHeader from "@/components/pp-header.vue";
import PpFilters from "@/components/pp-filters.vue";
import PpTodoList from "@/components/pp-todoList.vue";
import PpAddTodo from "@/components/pp-addTodo.vue";
import PpFooter, {Stats} from "@/components/pp-footer.vue";
import {Filter} from "@/types/Filter";

interface State {
  todos: Todo[],
  activeFilter: Filter
}

export default defineComponent({
  components: {PpFooter, PpAddTodo, PpTodoList, PpFilters, PpHeader},
  data(): State {
    return {
      todos: [
        {id: 0, text: 'Test', completed: true},
        {id: 1, text: 'Test 1', completed: false},
        {id: 2, text: 'Test 2', completed: false}
      ],
      activeFilter: 'All'
    }
  },
  computed: {
    filteredTodos(): Todo[] {
      switch (this.activeFilter) {
        case 'Active':
          return this.activeTodos
        case 'Done':
          return this.doneTodos
        case 'All':
        default:
          return this.todos
      }
    },
    stats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length,
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter(todo => todo.completed)
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

<style lang="scss" scoped>

</style>