<template>
  <v-container>
    <transition-group name="slideDown" appear tag="div" class="list-letters">
      <v-card color="red" v-for="index in wordLength" :key="index" outlined>
      </v-card>
      <v-row justify="center">
        <v-dialog v-model="selectedWord" width="600px">
          <v-card>
            <v-card-title class="justify-center">
              Cette lettre fait partie de votre mot ?
            </v-card-title>
            <h1 class="text-center">H</h1>
            <v-card-actions class="justify-center">
              <v-btn class="primary" rounded @click="Validate()"> Oui </v-btn>
              <v-btn class="error" rounded @click="Decline()"> Non </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </transition-group>
  </v-container>
</template>


<script>
export default {
  name: "GameRoom",

  data: () => ({
    selectedWord: true
  }),

  props: ["wordLength"],

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
