<template>
  <v-container>
    <v-row no-gutters justify="space-between">
      <v-col cols="auto" class="mr-5">
        <v-btn @click="openForm" color="success" class="mr-3">
          Создать новый лист
          <v-icon right>{{ icons.mdiPlus }}</v-icon>
        </v-btn>
        <v-btn @click="clearList" color="error">
          Удалить все
          <v-icon right>{{ icons.mdiDelete }}</v-icon>
        </v-btn>
      </v-col>
      <v-col class="d-flex" cols="3">
        <v-text-field v-model="search" label="Search for..." solo dense hide-details></v-text-field>
        <v-btn @click="resetSearch" color="primary" outlined height="40">{{ buttonText }}</v-btn>
      </v-col>
    </v-row>

    <todo-new-item
      :isCreating="isCreating"
      @createTodo="createTodo"
      @closeForm="closeForm"
      v-show="isCreating"
    />

    <v-row no-gutters>
      <v-col cols="12">
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>
              <v-chip class="ma-2" color="success" label>
                <v-icon left>{{ icons.mdiCheckAll }}</v-icon>
                Completed Tasks: {{ todos.filter(todo => {return todo.done === true}).length }}
              </v-chip>
            </v-list-item-title>
            <v-list-item-title>
              <v-chip class="ma-2" color="warning" label>
                <v-icon left>{{ icons.mdiAccountClockOutline }}</v-icon>
                Pending Tasks: {{ todos.filter(todo => {return todo.done === false}).length }}
              </v-chip>
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-col>

      <v-col>
        <todo-item
          @deleteTodo="deleteTodo"
          @completeTodo="completeTodo"
          v-for="(todo, i) in filteredList"
          :key="i"
          :todo="todo"
          :icons="icons"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import {
  mdiPlus,
  mdiPencil,
  mdiDelete,
  mdiCheckAll,
  mdiAccountClockOutline,
  mdiContentSaveEditOutline,
  mdiFileCancelOutline,
  mdiClose
} from "@mdi/js";
import TodoItem from "@/components/TodoItem";
import TodoNewItem from "@/components/TodoNewItem";

export default {
  name: "TodoList",

  props: {
    todos: {
      type: Array,
      required: false,
      default: () => []
    }
  },

  components: {
    TodoItem,
    TodoNewItem
  },

  data: () => ({
    search: "",
    icons: {
      mdiPlus,
      mdiPencil,
      mdiDelete,
      mdiCheckAll,
      mdiAccountClockOutline,
      mdiContentSaveEditOutline,
      mdiFileCancelOutline,
      mdiClose
    },
    isCreating: false
  }),

  computed: {
    filteredList() {
      return this.todos.filter(todo => {
        return todo.title.toLowerCase().includes(this.search.toLowerCase());
      });
    },
    buttonText() {
      return this.search.length === 0 ? 'Go!' : 'X'
    }
  },

  methods: {
    deleteTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos.splice(todoIndex, 1);
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
    },
    clearList() {
      this.$emit("delete");
    },
    openForm() {
      this.isCreating = true;
    },
    closeForm(el) {
      this.isCreating = el;
    },
    createTodo(title) {
      this.$emit("createItem", title);
    },
    resetSearch() {
      this.search = this.search.length > 0 ? '' : this.search
    }
  }
};
</script>
