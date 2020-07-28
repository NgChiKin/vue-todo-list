<template>
  <section class="real-app">
    <input
      type="text"
      class="add-input"
      autofocus
      placeholder="I want to do someting?"
      @keyup.enter="addTodo"
    />
    <Item v-for="item in filterdToods" :key="item.id" :todo="item" @del="deleteTodo" />
    <Tabs :filter="filter" :todos="todos" @toggle="toggleFilter" @clearAllCompleted="clearAllCompleted" />
  </section>
</template>

<script>
import Item from "./item.vue";
import Tabs from "./tabs.vue";
let id = 0;
export default {
  components: {
    Item,
    Tabs,
  },
  data() {
    return {
      todo: {
        id: 0,
        content: "this is todo",
        completed: false,
      },
      todos: [],
      filter: "all",
    };
  },
  methods: {
    addTodo(e) {
      if(!e.target.value.trim())
        return
      this.todos.unshift({
        id: id++,
        content: e.target.value.trim(),
        completed: false,
      });
      e.target.value = "";
    },
    deleteTodo(id) {
      this.todos.splice(
        this.todos.findIndex((value) => value.id === id),
        1
      );
    },
    toggleFilter(state) {
      this.filter = state;
    },
    clearAllCompleted() {
      this.todos = this.todos.filter(item => !item.completed)
    },
  },
  computed: {
    filterdToods() {
      if (this.filter === "all") return this.todos;
      const completed = this.filter === "completed";
      return this.todos.filter((value) => completed === value.completed);
    },
  },
};
</script>

<style lang="stylus" scoped>
.real-app {
  width: 600px;
  margin: 0 auto;
  box-shadow: 0 0 5px #666;
}

.add-input {
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  border: 0;
  outline: none;
  color: inherit;
  padding: 6px;
  border: 1px solid #999;
  box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  box-sizing: border-box;
  font-smoothing: antialiased;
  padding: 16px 16px 16px 60px;
  border: none;
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}
</style>