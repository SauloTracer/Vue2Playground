<template>
  <v-card>
    <v-card-title>DateDiff</v-card-title>
    <v-card-text>
      <v-form ref="form" v-on:submit.prevent="() => {}">
        <v-row>
          <v-col cols="4">
            <v-text-field
              label="Start"
              v-model="startDate"
              :rules="rules.obrigatorio"
              type="datetime-local"
            ></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-text-field
              label="End"
              v-model="endDate"
              :rules="rules.obrigatorio"
              type="datetime-local"
            ></v-text-field>
          </v-col>
          <v-col cols="2">
            <v-btn color="orange" class="mx-2" @click="calc()">
              <v-icon dark> mdi-equal </v-icon>
            </v-btn>
          </v-col>
          <v-col cols="2">
            <v-btn color="red" class="mx-2" @click="clear()">
              <v-icon dark> mdi-close </v-icon>
            </v-btn>
          </v-col>
        </v-row>
      </v-form>
      <v-row v-if="result">
        <v-spacer></v-spacer>
        <v-col cols="12">
          <h2>
            {{ result.totalDias ? `${result.totalDias} dia(s)` : "" }}
            {{ result.horas ? `${result.horas} hora(s)` : "" }}
            {{ result.minutos ? `${result.minutos} minuto(s)` : "" }}
          </h2>
          <h3 v-if="result.totalDias >= 30">
            ~ {{ result.anos ? `${result.anos} ano(s)` : "" }}
            {{ result.meses ? `${result.meses} mese(s)` : "" }}
            {{ result.dias ? `${result.dias} dia(s)` : "" }}
            {{ result.horas ? `${result.horas} hora(s)` : "" }}
            {{ result.minutos ? `${result.minutos} minuto(s)` : "" }}
          </h3>
        </v-col>
        <v-spacer></v-spacer>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      startDate: 0,
      endDate: 0,
      result: {
        anos: 0,
        meses: 0,
        dias: 0,
        totalDias: 0,
        horas: 0,
        minutos: 0,
      },
      rules: {
        obrigatorio: [(value) => !!value || "Preenchimento obrigatório!"],
      },
    };
  },
  methods: {
    calc() {
      let startDate = new Date(this.startDate);
      let endDate = new Date(this.endDate);

      let totalMs = endDate - startDate;
      let remainingMs = totalMs;

      let msDay = 86400000;
      let msHour = 3600000;
      let msMinute = 60000;

      let dias = Math.floor(totalMs / msDay); //Um dia em milisegundos
      let totalDias = dias;
      remainingMs -= dias * msDay;

      let horas = Math.floor(remainingMs / msHour);
      remainingMs -= horas * msHour;

      let minutos = Math.floor(remainingMs / msMinute);

      let anos = Math.floor(dias / 365);
      dias -= anos * 365;

      let meses = Math.floor(dias / 30);
      dias -= meses * 30;

      this.result = { anos, meses, dias, totalDias, horas, minutos };
    },
    clear() {
      this.$refs.form.reset();
      this.result = {
        anos: 0,
        meses: 0,
        dias: 0,
        totalDias: 0,
        horas: 0,
        minutos: 0,
      };
    },
  },
  mounted() {
    this.clear();
  },
};
</script>

<style scoped>
</style>