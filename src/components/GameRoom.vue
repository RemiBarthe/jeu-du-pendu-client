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
  </v-container>
</template>

<script>
export default {
  name: "GameRoom",

  data: () => ({})
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
}
</style>
