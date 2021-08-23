<template>
  <div id="app">
    <div id="main" class="content">
      <v-row>
        <v-col cols="6">
          <timers v-show="timers" />
        </v-col>
        <v-col cols="6">
          <todo v-show="todo" />
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import timers from "./components/timers";
import todo from "./components/todo";

export default {
  name: "Timer",
  components: { todo, timers },
  data() {
    return {
      time: 10,
      message: "",
      comps: [],
      timers: true,
      todo: true,
    };
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