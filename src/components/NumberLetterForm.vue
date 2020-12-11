<template>
  <v-container>
    <h2>Combien de lettres dans le mot ? (2-21)</h2>

    <p class="size-number">{{ number }}</p>

    <v-btn large color="error" class="ma-2" @click="decrementNumber">
      <v-icon dark> mdi-minus </v-icon></v-btn
    >
    <v-btn large color="primary" class="ma-2" @click="incrementNumber">
      <v-icon dark> mdi-plus </v-icon></v-btn
    >

    <v-btn large color="success" class="ma-2" @click="onSubmit">
      Valider
    </v-btn>
  </v-container>
</template>
<script>
export default {
  name: "NumberLetterForm",

  data: () => ({
    number: 2
  }),

  methods: {
    incrementNumber() {
      if (this.number < 21) {
        this.number++;
      }
    },
    decrementNumber() {
      if (this.number > 2) {
        this.number--;
      }
    },
    onSubmit() {
      this.$emit("submitted");
    }
  }
};
</script>
<style>
.size-number {
  font-size: 75px;
}
</style>