<template>
  <v-card>
    <v-card-title>Timer</v-card-title>

    <v-card-text>
      <v-spacer></v-spacer>
      <v-row>
        <v-col cols="2">
          <v-text-field v-model="title" label="Title" />
        </v-col>
        <v-col cols="2">
          <v-text-field v-model="time" type="number" label="Seconds" />
        </v-col>
        <v-col cols="1">
          <v-btn @click="addTimer" primary>START</v-btn>
        </v-col>
      </v-row>
      <v-spacer></v-spacer>
      <!-- Concluídos -->
      <div ref="container"></div>
      <!-- Em andamento -->
    </v-card-text>
  </v-card>
</template>

<script>
import Vue from "vue";
import timer from "./timer";

export default {
  name: "Timer",
  data() {
    return {
      time: 60,
      title: "",
      message: "",
      comps: [],
    };
  },
  methods: {
    addTimer() {
      var ComponentClass = Vue.extend(timer);
      var instance = new ComponentClass({
        propsData: {
          time: Number(this.time),
          message: String(this.message),
          title: String(this.title),
        },
      });
      instance.$mount();
      instance.$on("close", () =>
        this.$refs.container.removeChild(instance.$el)
      );
      this.$refs.container.appendChild(instance.$el);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input {
  width: 3em;
}
</style>