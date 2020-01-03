<template>
  <v-card>
    <v-list-item v-show="!isEditing">
      <v-list-item-content>
        <v-list-item-title>
          <v-chip @click="completeTodo(todo)" color="warning" v-show="!isEditing && !todo.done">
            <v-icon>{{ icons.mdiAccountClockOutline }}</v-icon>
          </v-chip>
          <v-chip color="success" v-show="!isEditing && todo.done">
            <v-icon>{{ icons.mdiCheckAll }}</v-icon>
          </v-chip>
          {{ todo.title }}
          <v-btn @click="showForm" color="warning" class="ml-5 mr-3">
            Редактировать
            <v-icon right>{{ icons.mdiPencil }}</v-icon>
          </v-btn>
          <v-btn @click="deleteTodo(todo)" color="error">
            Удалить
            <v-icon right>{{ icons.mdiDelete }}</v-icon>
          </v-btn>
        </v-list-item-title>
      </v-list-item-content>
    </v-list-item>

    <v-list-item v-show="isEditing">
      <v-list-item-content>
        <v-form class="d-flex">
          <v-text-field v-model="todo.title" solo dense hide-details label="Title" />
          <v-btn @click="hideForm" color="success" class="ml-5 mr-3">
            Сохранить
            <v-icon right>{{ icons.mdiContentSaveEditOutline }}</v-icon>
          </v-btn>
          <v-btn @click="cancelChanges" color="error">
            Отмена
            <v-icon right>{{ icons.mdiFileCancelOutline }}</v-icon>
          </v-btn>
        </v-form>
      </v-list-item-content>
    </v-list-item>
  </v-card>
</template>

<script>
export default {
  name: "TodoItem",

  props: {
    todo: {
      type: Object,
      required: false,
      default: () => {}
    },
    icons: {
      type: Object,
      required: false,
      default: () => {}
    }
  },

  data: () => ({
    isEditing: false
  }),

  methods: {
    showForm() {
      this.isEditing = true;
    },
    hideForm() {
      this.isEditing = false;
    },
    cancelChanges() {
      document.location.reload(true);
    },
    deleteTodo(todo) {
      this.$emit("deleteTodo", todo);
    },
    completeTodo(todo) {
      this.$emit("completeTodo", todo);
    }
  }
};
</script>