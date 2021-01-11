<template>
  <div>
    <NumberLetterForm @submitted="isSubmit" v-if="!word" />
    <GameRoom @submitted="isSubmit" :word="word" v-else />
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
      this.$emit("submitted");
      setTimeout(() => {
        this.word = number;
      }, 500);
    }
  }
};
</script>