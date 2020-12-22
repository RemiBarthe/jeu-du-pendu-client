<template>
  <v-container>
    <h1 v-if="tittleWhereIsLetter" class="style-color-titre">
      Où se trouve la lettre
      <span class="letter-color">{{ letterPossible }}</span>
      dans votre mot ?
    </h1>
    <transition-group name="fadeLeft" appear tag="div" class="list-letters">
      <v-card
        @click="selectLetter(letter.position)"
        :class="{ red: !letter.value, blue: letter.value }"
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
      >Valider
    </v-btn>
    <v-row justify="center">
      <v-dialog v-model="modalLetterChoose" width="600px" persistent>
        <v-card>
          <v-card-title class="justify-center">
            Cette lettre fait partie de votre mot ?
          </v-card-title>
          <h1 class="text-center uppercase-modal">{{ letterPossible }}</h1>
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

    <v-row justify="center">
      <v-dialog v-model="modalVictory" width="600px">
        <v-card>
          <v-card-title class="justify-center"> Facile ! </v-card-title>
          <h1 class="text-center uppercase-modal">
            Ton mot est {{ findedWord }} je l'ai trouvé en
            {{ countGuess }} coups
          </h1>
          <v-icon color="primary" @click="restart">mdi-restart</v-icon>
        </v-card>
      </v-dialog>
    </v-row>

    <v-row justify="center">
      <v-dialog v-model="modalDefeat" width="600px" persistent>
        <v-card>
          <v-card-title class="justify-center"> Oups..! </v-card-title>
          <h1 class="text-center uppercase-modal">Je ne connais pas ton mot</h1>
          <v-text-field placeholder="C'est quoi ton mot ?">TOTO</v-text-field>
          <v-btn color="success">Ajouter le mot</v-btn>
          <v-icon color="primary" @click="restart">mdi-restart</v-icon>
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
    modalVictory: false,
    modalDefeat: false,
    tittleWhereIsLetter: false,
    letterPossible: "",
    letters: "",
    positions: "",
    askedLetter: [],
    findedWord: "",
    countGuess: 0
  }),

  props: ["word"],

  created() {
    setTimeout(() => {
      this.askLetter("e");
    }, 1200);
  },

  methods: {
    modalValidateLetter() {
      this.modalLetterChoose = false;
      this.tittleWhereIsLetter = true;
    },
    modalDeclineLetter() {
      this.noLettersPosition();
      this.getWords();
    },
    confirmLetter() {
      this.tittleWhereIsLetter = false;
      this.getWords();
    },
    askLetter(letter) {
      this.modalLetterChoose = true;
      this.letterPossible = letter;
      this.askedLetter.push(letter);
    },
    selectLetter(position) {
      if (this.word[position - 1].value === this.letterPossible) {
        this.word[position - 1].value = "";
      } else {
        this.word[position - 1].value = this.letterPossible;
      }
    },
    noLettersPosition() {
      if (this.letters == "") {
        this.letters += this.letterPossible;
        this.positions += 0;
      } else {
        this.letters += "," + this.letterPossible;
        this.positions += "," + 0;
      }
    },
    addToLettersPositions() {
      this.word.forEach((letter) => {
        if (letter.value === this.letterPossible) {
          if (this.letters == "") {
            this.letters += this.letterPossible;
            this.positions += letter.position;
          } else {
            this.letters += "," + this.letterPossible;
            this.positions += "," + letter.position;
          }
        }
      });
    },
    getWords() {
      this.addToLettersPositions();
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
          if (response.data.length == 0) {
            this.modalDefeat = true;
          } else {
            if (response.data.length > 1) {
              this.getPopularLetter(response.data);
              this.countGuess++;
            } else {
              console.log(response.data);
              this.findedWord = response.data[0].label;
              this.modalVictory = true;
            }
          }
        });
    },
    getPopularLetter(response) {
      let allWord = "";

      response.forEach((word) => {
        allWord += word.label;
      });

      this.askedLetter.forEach((letter) => {
        allWord = allWord.split(letter).join("");
      });

      this.askLetter(this.maxChar(allWord));
    },
    maxChar(str) {
      const charMap = {};
      let max = 0;
      let maxChar = "";

      for (let char of str) {
        if (charMap[char]) {
          charMap[char]++;
        } else {
          charMap[char] = 1;
        }
      }

      for (let char in charMap) {
        if (charMap[char] > max) {
          max = charMap[char];
          maxChar = char;
        }
      }

      return maxChar;
    },
    restart() {
      this.$emit("submitted", null);
    }
  }
};
</script>

<style scoped>
.style-color-titre {
  color: white;
}
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
  text-transform: uppercase;
}

.list-letters p {
  font-size: 50px;
  text-align: center;
  margin: 0;
}
.letter-color {
  color: red;
  text-transform: uppercase;
}
.uppercase-modal {
  text-transform: uppercase;
}
</style>
