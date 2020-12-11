<template>
  <v-app>
    <v-main>
      <GameRoomContainer />
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
