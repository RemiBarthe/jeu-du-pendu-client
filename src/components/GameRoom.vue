<template>
  <div>
    <v-container class="container-gameroom">
      <div class="life-container">
        <h3 class="letter-title">
          Nombre de vie de l'ordinateur
        </h3>

        <div class="life">
          <v-icon
            v-for="(lifeLeft, index) in life"
            :key="index"
            color="#007cc7"
            class="ma-1"
          >
            mdi-heart
          </v-icon>

          <v-icon
            v-for="(life, index) in lifeMissing"
            :key="'missing' + index"
            color="#203647"
            class="ma-1"
          >
            mdi-heart
          </v-icon>
        </div>
      </div>

      <h1 v-if="tittleWhereIsLetter" class="letter-title">
        Où se trouve la lettre
        <span class="letter-color">{{ letterPossible }}</span>
        dans votre mot ?
      </h1>
      <transition-group name="slideUp" appear tag="div" class="list-letters">
        <v-card
          @click="selectLetter(letter.position)"
          :class="{ empty: !letter.value }"
          v-for="letter in word"
          :key="letter.position"
          dark
          elevation="0"
        >
          {{ letter.value }}
        </v-card>
      </transition-group>

      <v-btn
        v-if="tittleWhereIsLetter"
        color="#4DA8DA"
        class="align-center"
        @click="confirmLetter"
        dark
        elevation="0"
        >Valider
      </v-btn>
    </v-container>

    <v-row justify="center">
      <v-dialog v-model="modalLetterChoose" width="600px" persistent>
        <v-card class="pa-8">
          <v-card-title class="justify-center">
            Cette lettre fait partie de votre mot ?
          </v-card-title>

          <h1 class="text-center uppercase-modal ma-3">
            {{ letterPossible }}
          </h1>

          <v-card-actions class="justify-center">
            <v-btn
              color="#4DA8DA"
              fab
              small
              @click="modalValidateLetter"
              elevation="0"
              dark
            >
              <v-icon dark> mdi-check </v-icon>
            </v-btn>

            <v-btn
              color="#203647"
              fab
              small
              dark
              @click="modalDeclineLetter"
              elevation="0"
            >
              <v-icon dark> mdi-close </v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="modalDefeat" width="600px" persistent>
        <v-card class="pa-8">
          <v-card-title class="justify-center"> Oups..! </v-card-title>
          <h1 class="text-center">
            {{ looseMessage }}
          </h1>

          <v-text-field
            v-model="newWord"
            :rules="[
              () =>
                formattedNewWord.length >= word.length ||
                'Votre mot doit faire ' + word.length + ' lettres'
            ]"
            placeholder="Quel est ton mot ?"
            counter
            :maxlength="word.length"
          ></v-text-field>

          <v-card-actions class="justify-center">
            <v-btn
              @click="addNewWord"
              :disabled="this.formattedNewWord.length !== this.word.length"
              color="#4DA8DA"
              elevation="0"
              class="ma-1 white--text"
              >Ajouter le mot</v-btn
            >

            <v-tooltip right open-delay="500">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  @click="restart"
                  v-bind="attrs"
                  v-on="on"
                  dark
                  color="#203647"
                  elevation="0"
                  class="ma-1"
                >
                  <v-icon>mdi-restart</v-icon>
                </v-btn>
              </template>
              <span>Rejouer</span>
            </v-tooltip>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="modalVictory" width="600px" persistent>
        <v-card class="pa-8">
          <v-card-title class="justify-center">
            {{ victoryMessage }}
          </v-card-title>

          <h1 class="text-center uppercase-modal">
            Ton mot est <span class="letter-color">{{ findedWord }}</span>
          </h1>

          <v-card-title class="justify-center">
            Je l'ai trouvé en {{ countGuess }} coups
          </v-card-title>

          <v-card-actions class="justify-center">
            <v-btn
              @click="wrongWord"
              color="error"
              elevation="0"
              class="ma-1 white--text"
              >C'EST FAUX !</v-btn
            >
            <v-tooltip right open-delay="500">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  @click="restart"
                  v-bind="attrs"
                  v-on="on"
                  dark
                  color="#203647"
                  elevation="0"
                  class="ma-1"
                >
                  <v-icon>mdi-restart</v-icon>
                </v-btn>
              </template>
              <span>Rejouer</span>
            </v-tooltip>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="modalSubmittedWord" width="600px" persistent>
        <v-card class="pa-8">
          <v-card-title class="justify-center">
            Votre mot sera ajouté prochainement
          </v-card-title>

          <h1 class="text-center uppercase-modal">Merci !</h1>

          <v-card-actions class="justify-center">
            <v-tooltip right open-delay="500">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  @click="restart"
                  v-bind="attrs"
                  v-on="on"
                  dark
                  color="#203647"
                  elevation="0"
                  class="ma-1"
                >
                  <v-icon>mdi-restart</v-icon>
                </v-btn>
              </template>
              <span>Rejouer</span>
            </v-tooltip>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>


<script>
import axios from "axios";
export default {
  name: "GameRoom",
  props: ["word"],
  data: () => ({
    modalLetterChoose: false,
    modalVictory: false,
    modalDefeat: false,
    modalSubmittedWord: false,
    tittleWhereIsLetter: false,
    letterPossible: "",
    letters: "",
    positions: "",
    askedLetter: [],
    findedWord: "",
    totalLife: 5,
    life: 5,
    countGuess: 1,
    newWord: ""
  }),
  created() {
    setTimeout(() => {
      this.askLetter("e");
    }, 1400);
  },
  computed: {
    formattedNewWord() {
      return this.newWord
        .toLowerCase()
        .normalize("NFD")
        .replace(/[\u0300-\u036f]/g, "")
        .replace(/[^\w\s]/gi, "");
    },
    looseMessage() {
      if (this.life === 0) {
        return "Incroyable tu as réussis à me battre. Aide-moi à en apprendre plus";
      }
      return "Je ne connais pas ton mot";
    },
    victoryMessage() {
      switch (this.life) {
        case 5:
          return "PERFECT GAME !!!";
        case 4:
          return "Victoire !";
        case 3:
          return "Facile !";
        case 2:
          return "Pas si simple";
      }
      return "Oulà, j'ai eu chaud !";
    },
    lifeMissing() {
      return this.totalLife - this.life;
    }
  },
  methods: {
    modalValidateLetter() {
      this.modalLetterChoose = false;
      this.tittleWhereIsLetter = true;
    },
    modalDeclineLetter() {
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
        if (!this.word[position - 1].value) {
          this.word[position - 1].value = this.letterPossible;
        }
      }
    },
    addToLettersPositions() {
      let count = 0;
      this.word.forEach((letter) => {
        if (letter.value === this.letterPossible) {
          if (this.letters == "") {
            this.letters += this.letterPossible;
            this.positions += letter.position;
          } else {
            this.letters += "," + this.letterPossible;
            this.positions += "," + letter.position;
          }
          count++;
        }
      });
      if (!count) {
        this.life--;
        if (this.letters == "") {
          this.letters += this.letterPossible;
          this.positions += 0;
        } else {
          this.letters += "," + this.letterPossible;
          this.positions += "," + 0;
        }
      }
    },
    getWords() {
      this.addToLettersPositions();
      axios
        .get(
          "http://jeudupenduapi-env.eba-jkmp4qhj.eu-west-3.elasticbeanstalk.com/words?length=" +
            this.word.length +
            "&letters=" +
            this.letters +
            "&positions=" +
            this.positions
        )
        .then((response) => {
          if (response.data.length === 0 || this.life <= 0) {
            this.modalDefeat = true;
          } else {
            if (response.data.length > 1) {
              this.getPopularLetter(response.data);
              this.countGuess++;
            } else {
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
    },
    addNewWord() {
      axios
        .get(
          "http://jeudupenduapi-env.eba-jkmp4qhj.eu-west-3.elasticbeanstalk.com/words/" +
            this.formattedNewWord
        )
        .then((response) => {
          if (!response.data.length) {
            axios.post(
              "http://jeudupenduapi-env.eba-jkmp4qhj.eu-west-3.elasticbeanstalk.com/words?label=" +
                this.formattedNewWord
            );
          }
          this.modalDefeat = false;
          this.modalLetterChoose = false;
          this.modalSubmittedWord = true;
        });
    },
    wrongWord() {
      this.modalVictory = false;
      this.modalDefeat = true;
    }
  }
};
</script>

<style scoped>
.container-gameroom {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column;
}
.letter-title {
  color: white;
  text-align: center;
}
.container {
  height: 100vh;
}
.list-letters {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px 0;
}
.list-letters .v-card {
  margin: 5px;
  width: 80px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
  background-color: #eda134;
  font-size: 25px;
}
.list-letters .v-card.empty {
  background-color: #203647;
}
.list-letters p {
  font-size: 50px;
  text-align: center;
  margin: 0;
}
.letter-color {
  color: #007cc7;
  text-transform: uppercase;
}
.uppercase-modal {
  text-transform: uppercase;
}
.life-container {
  width: 100%;
  position: absolute;
  top: 40px;
}
.life {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
