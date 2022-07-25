<template>
  <v-card>
    <v-card-title>Percent (%)</v-card-title>
    <v-card-text>
      <v-form ref="form" v-on:submit.prevent="() => {}">
        <v-row>
          <v-col cols="2">
            <v-text-field
              label="Part"
              v-model="part"
              :rules="rules.obrigatorio"
              type="number"
            ></v-text-field>
          </v-col>
          <v-col cols="2">
            <v-text-field
              label="Total"
              v-model="total"
              :rules="rules.obrigatorio"
              type="number"
            ></v-text-field>
          </v-col>
          <v-col cols="2">
            <v-text-field
              label="Steps"
              v-model="steps"
              type="number"
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
      <v-row v-if="res">
        <v-spacer></v-spacer>
        <v-col cols="2">
          <h2>{{ res.toFixed(2) }} %</h2>
        </v-col>
        <v-spacer></v-spacer>
      </v-row>
      <v-row v-if="res">
        <v-spacer></v-spacer>
        <h3>{{ total - part }} remaining</h3>
        <v-spacer></v-spacer>
      </v-row>
      <v-row v-if="targetStep">
        <v-col cols="12">
          <h3>Step target: {{ targetStep.toFixed(2) }}</h3>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      part: 0,
      total: 0,
      steps: 0,
      res: 0,
      targetStep: 0,
      rules: {
        obrigatorio: [(value) => !!value || "Preenchimento obrigatório!"],
      },
    };
  },
  methods: {
    calc() {
      this.res = (100 * this.part) / this.total;
      if (this.steps) this.targetStep = (this.total - this.part) / this.steps;
    },
    clear() {
      this.$refs.form.reset();
      this.res = 0;
      this.targetStep = 0;
    },
  },
  mounted() {
    this.clear();
  },
};
</script>

<style scoped>
</style>