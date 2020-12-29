<template>
  <v-app>
    <v-main class="background">
      <div class="app-background">
        <div class="liquid" :class="{ animate: animate }"></div>
      </div>
      <GameRoomContainer
        class="gamecontainer"
        @submitted="pageTransition"
        v-show="!animate"
      />
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
    animate: false
  }),
  methods: {
    pageTransition() {
      this.animate = true;
      setTimeout(() => {
        this.animate = false;
      }, 500);
    }
  }
};
</script>
<style scoped>
.gamecontainer {
  position: relative;
  z-index: 2;
}

.background {
  background-color: #007cc7;
}
.app-background {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  overflow: hidden;
  z-index: 1;
}

.app-background .liquid {
  position: relative;
  top: -60%;
  width: 2400px;
  height: 2400px;
  transition: 0.5s;
  z-index: 1;
}

.app-background .animate {
  top: -150%;
  z-index: 99;
}

.app-background .liquid::after,
.app-background .liquid::before {
  content: "";
  width: 200%;
  height: 200%;
  position: absolute;
  top: 0;
  left: 45%;
  transform: translate(-50%, -75%);
  background: #000;
  z-index: 1;
}

.app-background .liquid::before {
  border-radius: 45%;
  background: rgba(18, 35, 46, 1);
  animation: animate 50s linear infinite;
  z-index: 1;
}

.app-background .liquid::after {
  border-radius: 40%;
  background: rgba(18, 35, 46, 0.5);
  animation: animate 100s linear infinite;
  z-index: 1;
}

@keyframes animate {
  0% {
    transform: translate(-50%, -75%) rotate(0deg);
  }
  100% {
    transform: translate(-50%, -75%) rotate(360deg);
  }
}
</style>