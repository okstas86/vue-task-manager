<template>
  <div>
    <base-dialog
      v-if="inputIsInvalid"
      title="Invalid Input"
      @close="confirmError"
    >
      <template #default>
        <p>Unfortunately, at least one input value is invalid.</p>
        <p>
          Please check all inputs and make sure you enter at least a few
          characters into each input field.
        </p>
      </template>
      <template #actions>
        <base-button @click="confirmError">Ok</base-button>
      </template>
    </base-dialog>
    <base-card>
      <form @submit.prevent="submitData" action="#">
        <div class="form-control">
          <label for="Title">Title</label>
          <input id="title" name="title" type="text" v-model="task.title" />
        </div>
        <div class="form-control">
          <label for="Description">Description</label>
          <textarea
            id="description"
            name="description"
            rows="3"
            v-model="task.description"
          ></textarea>
        </div>

        <div>
          <base-button type="submit">Add Task</base-button>
        </div>
      </form>
    </base-card>
  </div>
</template>

<script>
export default {
  name: "FormList",
  data() {
    return {
      task: {
        title: "",
        description: "",
      },
      inputIsInvalid: false,
    };
  },
  computed: {
    isFormValid() {
      return (
        this.task.title.trim() !== "" && this.task.description.trim() !== ""
      );
    },
  },
  methods: {
    submitData() {
      if (!this.isFormValid) {
        this.inputIsInvalid = true;
        return;
      }

      const task = {
        title: this.task.title,
        description: this.task.description,
      };
      this.task.title = "";
      this.task.description = "";
      this.$store.dispatch("addTask", task);
      this.$router.push("/tasks");
    },
    confirmError() {
      this.inputIsInvalid = false;
    },
  },
};
</script>
<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
