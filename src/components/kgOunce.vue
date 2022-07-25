<template>
  <v-card>
    <v-card-title>Percent (%)</v-card-title>

    <v-card-text>
      <v-row>
        <v-col cols="5">
          <v-text-field
            v-model="kg"
            type="number"
            label="KG"
            @keypress.enter="convert"
          />
        </v-col>
        <v-col cols="2">
          <v-icon>mdi-swap-horizontal</v-icon>
        </v-col>
        <v-col cols="5">
          <v-text-field
            v-model="ounce"
            type="number"
            label="Ounce"
            @keypress.enter="convert"
          />
        </v-col>
      </v-row>
    </v-card-text>

    <v-card-actions>
      <v-btn @click="convert" primary ref="btnConvert">
        <v-icon>mdi-weight</v-icon>
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    kg: 0,
    ounce: 0,
  }),
  methods: {
    convert() {
      if (this.kg) {
        this.ounce = this.kg / 0.0283495;
      } else if (this.ounce) {
        this.kg = this.ounce * 0.0283495;
      }
    },
  },
  mounted() {},
};
</script>