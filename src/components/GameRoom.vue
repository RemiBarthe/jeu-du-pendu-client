<template>
  <v-container>
    <div class="list-letters">
      <v-card outlined>
        <p>C</p>
      </v-card>

      <v-card outlined>
        <p>H</p>
      </v-card>

      <v-card outlined>
        <p>A</p>
      </v-card>

      <v-card outlined>
        <p>I</p>
      </v-card>

      <v-card outlined>
        <p>S</p>
      </v-card>

      <v-card outlined>
        <p>E</p>
      </v-card>
    </div>
    <v-row justify="center">
      <v-dialog v-model="selectedWord" width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
            @click="dialog = true"
          >
            test
          </v-btn>
        </template>
        <div align="center">
          <v-card width="370">
            <v-card-title>
              Cette lettre fait partie de votre mot ?
            </v-card-title>
            <h1 class="letter">H</h1>
            <v-card-actions>
              <v-spacer></v-spacer>
              <div class="align-center">
                <v-btn class="validateBtn" rounded outlined @click="Validate()">
                  Oui
                </v-btn>
                <v-btn class="declineBtn" rounded outlined @click="Decline()">
                  Non
                </v-btn>
              </div>
            </v-card-actions>
          </v-card>
        </div>
      </v-dialog>
    </v-row>
  </v-container>
</template>


<script>
export default {
  name: "GameRoom",

  data: () => ({
    selectedWord: true
  }),

  methods: {
    Validate() {
      console.log("oui");
      this.selectedWord = false;
    },
    Decline() {
      console.log("Non");
      this.selectedWord = false;
    }
  }
};
</script>

<style scoped>
.container {
  height: 100vh;
}

.list-letters {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.letter {
  margin: 30px;
}
.declineBtn {
  margin-right: 100px;
}
.validateBtn {
  margin-right: 25px;
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
</style>
