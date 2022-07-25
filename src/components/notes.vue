<template>
  <v-card>
    <v-card-title>Notes</v-card-title>
    <v-card-text>
      <v-form ref="form" v-on:submit.prevent="() => {}">
        <v-row>
          <v-col>
            <v-text-field
              label="Titulo"
              v-model="note.title"
              :rules="rules.obrigatorio"
              type="text"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              label="Conteúdo"
              v-model="note.content"
              :rules="rules.obrigatorio"
              type="number"
            ></v-textarea>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="2">
            <v-text-field
              label="Tags"
              v-model="note.tags"
              type="text"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="2">
            <v-btn color="orange" class="mx-2" @click="save()">
              <v-icon dark> mdi-save </v-icon>
            </v-btn>
          </v-col>
          <v-col cols="2">
            <v-btn color="red" class="mx-2" @click="clear()">
              <v-icon dark> mdi-close </v-icon>
            </v-btn>
          </v-col>
        </v-row>
      </v-form>

      <!-- 
        Listar notas organizadas por tags
      -->
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      notes: [],
      note: {
        id: 0,
        title: "",
        content: "",
        tags: [],
        links: [],
      },
      rules: {
        obrigatorio: [(value) => !!value || "Preenchimento obrigatório!"],
      },
    };
  },
  methods: {
    save() {
      this.notes.push(this.note);
      this.saveNotes();
      this.clear();
    },
    saveNotes() {
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },
    clear() {
      this.$refs.form.reset();
      this.note = {
        id: 0,
        title: "",
        content: "",
        tags: [],
        links: [],
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