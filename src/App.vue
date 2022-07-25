<template>
  <div id="app">
    <div id="main" class="content">
      <v-row>
        <v-col cols="6">
          <timers v-show="timers" />
          <br />
          <percent v-show="percent" />
          <br />
          <date-diff v-show="dateDiff" />
          <br />
          <notes v-show="notes" />
        </v-col>
        <v-col cols="6">
          <todo v-show="todo" />
          <br />
          <kg-ounce v-show="kgOunce" />
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import timers from "./components/timers";
import todo from "./components/todo";
import percent from "./components/percent";
import dateDiff from "./components/dateDiff";
import notes from "./components/notes";
import kgOunce from "./components/kgOunce.vue";

export default {
  name: "Tracer",
  components: { todo, timers, percent, dateDiff, notes, kgOunce },
  data() {
    return {
      time: 10,
      message: "",
      comps: [],
      timers: true,
      todo: true,
      percent: true,
      dateDiff: true,
      kgOunce: true,
      notes: true,
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