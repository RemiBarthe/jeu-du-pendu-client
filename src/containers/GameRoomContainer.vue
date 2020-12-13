<template>
  <div>
    <NumberLetterForm @submitted="isSubmit" v-if="!word" />
    <GameRoom :word="word" v-else />
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
    word: null
  }),
  methods: {
    isSubmit(number) {
      this.word = number;
    }
  }
};
</script>
