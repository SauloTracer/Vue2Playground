<template>
  <v-card>
    <v-card-title>X.T.D.L.</v-card-title>
    <v-card-text>
      <v-form ref="formTodo" v-on:submit.prevent="() => {}">
        <v-row>
          <v-col cols="4">
            <v-text-field
              label="New task"
              v-model="task.title"
              :rules="rules.obrigatorio"
            ></v-text-field>
          </v-col>
          <v-col cols="2">
            <v-btn color="blue" elevation="5" @click="addTask()">Add</v-btn>
          </v-col>
          <v-col cols="2">
            <v-btn color="blue" elevation="5" @click="checkAll()"
              >Check All</v-btn
            >
          </v-col>
          <v-col cols="2">
            <v-btn color="blue" elevation="5" @click="uncheckAll()"
              >Uncheck All</v-btn
            >
          </v-col>
        </v-row>
      </v-form>
      <v-row>
        <v-col cols="12">
          <ul>
            <li v-for="(el, index) in tasks" :key="index">
              <span v-bind:class="{ done: el.done }">
                {{ el.title }}
                <v-btn
                  icon
                  x-small
                  color="green"
                  class="mx-2"
                  @click="done(el)"
                >
                  <v-icon> mdi-check </v-icon>
                </v-btn>
                <v-btn
                  icon
                  x-small
                  color="red"
                  class="mx-2"
                  @click="deleteTask(el)"
                >
                  <v-icon dark> mdi-close </v-icon>
                </v-btn>
              </span>
            </li>
          </ul>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      task: {
        id: "",
        title: "",
        group: "",
        permanent: false,
        done: false,
        active: true,
        parent: null,
      },
      tasks: [], // Array of task objects
      rules: {
        obrigatorio: [(value) => !!value || "Preenchimento obrigatório!"],
      },
    };
  },
  created() {
    let savedTasks = localStorage.getItem("tasks");
    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks);
    }
  },
  methods: {
    addTask() {
      if (this.$refs.formTodo.validate()) {
        this.task.id = Date.now();
        this.task.group = this.task.group ? this.task.group : "Geral";
        this.tasks.push({ ...this.task });
        this.$refs.formTodo.reset();
        this.saveTasks();
      } else {
        console.log("Erros de validação no form.");
      }
    },
    findTask(target) {
      return this.tasks.find((x) => x.id == target.id);
    },
    done(target) {
      const task = this.findTask(target);
      if (task) {
        task.done = !task.done;
      }
      this.saveTasks();
    },
    deleteTask(target) {
      this.tasks = this.tasks.filter((x) => x.id != target.id);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    changeAll(value) {
      this.tasks.map((t) => (t.done = value));
      this.saveTasks();
    },
    checkAll() {
      this.changeAll(true);
    },
    uncheckAll() {
      this.changeAll(false);
    },
  },
};
</script>

<style scoped>
.done {
  text-decoration: line-through;
}

ul {
  list-style-type: none;
}
</style>