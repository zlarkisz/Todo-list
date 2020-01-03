<template>
  <v-app>
    <todo-list @delete="clearList" @createItem="createItem" :todos="todos" />
  </v-app>
</template>

<script>
import TodoList from "@/components/TodoList";
export default {
  name: "App",

  components: {
    TodoList
  },

  data: () => ({
    todos: [
      {
        title: "Todo me 1",
        done: false
      },
      {
        title: "Todo me 2",
        done: true
      },
      {
        title: "Todo me 3",
        done: false
      },
      {
        title: "Todo me 4",
        done: false
      }
    ]
  }),

  methods: {
    clearList() {
      while (this.todos.length) {
        this.todos.pop();
      }
    },
    createItem(item) {
      this.todos.push(item);
    }
  }
};
</script>
