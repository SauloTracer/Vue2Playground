<template>
  <v-card>
    <v-card-title>Timer</v-card-title>

    <v-card-text>
      <v-spacer></v-spacer>
      <v-row>
        <v-col cols="6">
          <v-text-field v-model="title" label="Title" />
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="2">
          <v-text-field v-model="horas" type="number" label="HH" />
        </v-col>
        <v-col cols="2">
          <v-text-field v-model="minutos" type="number" label="MM" />
        </v-col>
        <v-col cols="2">
          <v-text-field v-model="segundos" type="number" label="SS" />
        </v-col>
        <v-col cols="1">
          <v-btn @click="addTimer" primary>
            <v-icon>mdi-play</v-icon>
          </v-btn>
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
import vuetify from "../plugins/vuetify";
import timer from "./timer";

export default {
  name: "Timer",
  data() {
    return {
      horas: 0,
      minutos: 0,
      segundos: 0,
      title: "",
      message: "",
      comps: [],
    };
  },
  methods: {
    addTimer() {
      var ComponentClass = Vue.extend(timer);
      var instance = new ComponentClass({
        vuetify,
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
  computed: {
    time: function () {
      console.log(this.horas, this.minutos, this.segundos);
      let value = this.horas * 3600;
      value += this.minutos * 60;
      value += this.segundos * 1; // Sem o *1 ele interpretou os segundos como string e adicionou o número no final de forma que 1h e 1s viram 36001 segundos (10h 1s)
      return value;
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