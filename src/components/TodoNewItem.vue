<template>
  <v-row no-gutter justify="center">
    <v-col cols="auto" class="d-flex">
      <v-text-field
        v-model="titleText"
        style="max-width: 320px"
        ref="title"
        label="Описание..."
        solo
        dense
        hide-details
        autofocus
      />
      <v-btn @click="sendForm" color="primary" outlined height="38">Ok!</v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "TodoNewItem",

  props: {
    isCreating: {
      type: Boolean,
      required: false,
      default: false
    }
  },

  data: () => ({
    titleText: ""
  }),

  methods: {
    sendForm() {
      if (this.titleText.length > 0) {
        const title = this.titleText;
        this.$emit("createTodo", { title, done: false });
        this.$emit("closeForm", false);
      }
      this.titleText = "";
    }
  }
};
</script>