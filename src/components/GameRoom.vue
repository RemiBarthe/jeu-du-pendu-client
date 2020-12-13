<template>
  <v-container>
    <transition-group name="slideDown" appear tag="div" class="list-letters">
      <v-card color="red" v-for="index in wordLength" :key="index" outlined>
      </v-card>
    </transition-group>
  </v-container>
</template>

<script>
export default {
  name: "GameRoom",

  data: () => ({}),
  props: ["wordLength"]
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
