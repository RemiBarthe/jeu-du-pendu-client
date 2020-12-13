<template>
  <div>
    <NumberLetterForm @submitted="isSubmit" v-if="!wordLength" />
    <GameRoom :wordLength="wordLength" v-else />
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
    wordLength: 0
  }),
  methods: {
    isSubmit(number) {
      this.wordLength = number;
    }
  }
};
</script>
