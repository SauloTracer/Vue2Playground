<template>
  <v-row>
    <v-spacer></v-spacer>
    <v-col cols="10">
      <v-progress-linear
        class="mb-2 mx-4"
        :value="percentage"
        color="#0f0"
        background-color="#ddd"
        height="18"
        :reverse="$vuetify.rtl"
      >
        <span v-if="!finished" style="margin-right: 10px">
          {{ title }} {{ toTime(current) }}
        </span>
        <span>{{ message }}</span>
      </v-progress-linear>
    </v-col>
    <v-spacer></v-spacer>
    <v-col cols="2">
      <v-btn small icon color="green" class="mx-2" @click="startCountdown()">
        <v-icon> mdi-cached </v-icon>
      </v-btn>
      <v-btn small icon color="red" class="mx-2" @click="destroy()">
        <v-icon> mdi-close </v-icon>
      </v-btn>
    </v-col>
    <v-spacer></v-spacer>
  </v-row>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    time: {
      type: Number,
      required: true,
    },
    message: {
      type: String,
      required: false,
      default: "Timer concluído!",
    },
    title: {
      type: String,
      required: false,
      default: "",
    },
  },
  data: () => ({
    current: 0,
    start: undefined,
    percentage: 0,
    finished: false,
  }),
  methods: {
    now: function () {
      return (Date.now() / 1000) | 0;
    },
    toTime: function (seconds) {
      if (seconds > 3600 * 24) return seconds;
      return new Date(seconds * 1000).toISOString().substr(11, 8);
    },
    step: function (timestamp) {
      if (this.finished) return;
      if (this.start === undefined) this.start = timestamp;
      const elapsed = timestamp - this.start;

      if (elapsed >= this.time * 1000) {
        this.start = undefined;
        this.message = `Timer ${this.title} finalizado!`;

        const synth = window.speechSynthesis;
        synth.speak(
          new SpeechSynthesisUtterance(`Timer ${this.title} finalizado!`)
        );

        this.finished = true;
        return;
      }

      this.current = ((this.time * 1000 - elapsed) / 1000) | 0;
      this.percentage = 100 - (this.current * 100) / this.time;
      this.message = this.percentage.toFixed(2) + " %";
      requestAnimationFrame(this.step);
    },
    startCountdown: function () {
      this.start = undefined;
      this.finished = false;
      this.message = "";
      requestAnimationFrame(this.step);
    },
    destroy() {
      this.finished = true;
      this.$emit("close");
    },
  },
  mounted() {
    this.startCountdown();
  },
};
</script>
