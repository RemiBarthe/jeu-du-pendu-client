<template>
  <v-app>
    <v-main>
      <div class="main-color">
        <GameRoomContainer />
      </div>
    </v-main>
  </v-app>
</template>


<script>
import GameRoomContainer from "./containers/GameRoomContainer";

export default {
  name: "App",

  components: {
    GameRoomContainer
  },

  data: () => ({
    //
  })
};
</script>
<style scoped>
.main-color {
  background: linear-gradient(0deg, #874fee, #2775e9);
}
</style>