<template>
  <div>
    <NumberLetterForm @submitted="isSubmited = true" v-if="!isSubmited" />
    <GameRoom v-else />
  </div>
</template>

<script>
import GameRoom from "../components/GameRoom";
import NumberLetterForm from "../components/NumberLetterForm";

export default {
  name: "GameRoomContainer",

  components: {
    GameRoom,
    NumberLetterForm
  },

  data: () => ({
    isSubmited: false
  })
};
</script>
