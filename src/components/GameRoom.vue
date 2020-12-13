<template>
  <v-container>
    <h1 v-if="tittleWhereIsLetter">
      Où se trouve la lettre
      <span class="letter-color">{{ letterPossible }}</span>
      dans votre mot ?
    </h1>
    <transition-group name="slideDown" appear tag="div" class="list-letters">
      <v-card
        @click="selectLetter(letter.position)"
        color="red"
        v-for="letter in word"
        :key="letter.position"
        outlined
      >
        {{ letter.value }}
      </v-card>
    </transition-group>
    <v-btn
      v-if="tittleWhereIsLetter"
      color="success"
      class="align-center"
      @click="confirmLetter"
      >Valider</v-btn
    >
    <v-row justify="center">
      <v-dialog v-model="modalLetterChoose" width="600px" persistent>
        <v-card>
          <v-card-title class="justify-center">
            Cette lettre fait partie de votre mot ?
          </v-card-title>
          <h1 class="text-center">{{ letterPossible }}</h1>
          <v-card-actions class="justify-center">
            <v-btn class="primary" rounded @click="modalValidateLetter">
              Oui
            </v-btn>
            <v-btn class="error" rounded @click="modalDeclineLetter">
              Non
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </v-container>
</template>


<script>
import axios from "axios";
export default {
  name: "GameRoom",

  data: () => ({
    modalLetterChoose: false,
    tittleWhereIsLetter: false,
    letterPossible: "",
    letters: "",
    positions: ""
  }),

  props: ["word"],

  created() {
    setTimeout(() => {
      this.askLetter("E");
    }, 1200);
  },

  methods: {
    modalValidateLetter() {
      this.modalLetterChoose = false;
      this.tittleWhereIsLetter = true;
    },
    modalDeclineLetter() {
      this.addToLettersPositions(0);
      this.getWords();
    },
    confirmLetter() {
      this.tittleWhereIsLetter = false;
      this.getWords();
    },
    askLetter(letter) {
      this.modalLetterChoose = true;
      this.letterPossible = letter;
    },
    selectLetter(position) {
      this.word[position - 1].value = this.letterPossible;
      this.addToLettersPositions(position);
    },
    addToLettersPositions(position) {
      if (this.letters == "") {
        this.letters += this.letterPossible;
        this.positions += position;
      } else {
        this.letters += "," + this.letterPossible;
        this.positions += "," + position;
      }
    },
    getWords() {
      axios
        .get(
          "https://whispering-refuge-98843.herokuapp.com/words?length=" +
            this.word.length +
            "&letters=" +
            this.letters +
            "&positions=" +
            this.positions
        )
        .then((response) => {
          console.log(response);
          this.askLetter("B");
        });
    }
  }
};
</script>

<style scoped>
.container {
  height: 100vh;
}

.list-letters {
  height: 80%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.list-letters .v-card {
  margin: 5px;
  width: 80px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.list-letters p {
  font-size: 50px;
  text-align: center;
  margin: 0;
}
.letter-color {
  color: red;
}
</style>
