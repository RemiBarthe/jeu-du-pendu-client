<template>
  <v-container class="center-container">
    <p class="style-title">Combien de lettres dans le mot ? (2-21)</p>
    <div class="position-number">
      <p class="size-number">{{ number }}</p>

      <div class="position-buttons">
        <v-btn x-small fab color="info" class="ma-2" @click="incrementNumber">
          <v-icon dark> mdi-plus </v-icon>
        </v-btn>
        <v-btn x-small fab color="error" class="ma-2" @click="decrementNumber">
          <v-icon dark> mdi-minus </v-icon>
        </v-btn>
      </div>
    </div>
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
  margin-bottom: 0 !important;
  margin-right: 5px;
  font-size: 75px;
}
.center-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column;
  height: 100vh;
}
.position-number {
  display: flex;
  justify-content: center;
  align-items: center;
}

.position-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column;
}
.style-title {
  font-size: 40px;
  padding-bottom: 10px;
}
</style>