<template>
  <div>        
      <img src="https://d2a0gza273xfgz.cloudfront.net/35835/uploads/6e2cb9f8-93d4-43f5-ab16-436df4cf34fa_800_420.png" alt="Robson" class="robson-image" />
    </div>
  <div class="home">
    <div class="container">
      <div class="free-poker-header">
        <h1>Robson está aqui para <span>ajudar</span>.</h1>
      </div>
      <div class="start-game">
        <button class="button" :class="{ disabled: clickedStart }" @click="startGame()">
          <span v-if="!clickedStart">Criar Sala</span>
          <svg
              v-if="clickedStart"
              version="1.1"
              id="Layer_1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              width="24px"
              height="30px"
              viewBox="0 0 24 30"
              style="enable-background: new 0 0 50 50"
              xml:space="preserve"
          >
            <rect x="0" y="10" width="4" height="10" fill="#333" opacity="0.2">
              <animate attributeName="opacity" attributeType="XML" values="0.2; 1; .2" begin="0s" dur="0.6s"
                       repeatCount="indefinite"/>
              <animate attributeName="height" attributeType="XML" values="10; 20; 10" begin="0s" dur="0.6s"
                       repeatCount="indefinite"/>
              <animate attributeName="y" attributeType="XML" values="10; 5; 10" begin="0s" dur="0.6s"
                       repeatCount="indefinite"/>
            </rect>
            <rect x="8" y="10" width="4" height="10" fill="#333" opacity="0.2">
              <animate attributeName="opacity" attributeType="XML" values="0.2; 1; .2" begin="0.15s" dur="0.6s"
                       repeatCount="indefinite"/>
              <animate attributeName="height" attributeType="XML" values="10; 20; 10" begin="0.15s" dur="0.6s"
                       repeatCount="indefinite"/>
              <animate attributeName="y" attributeType="XML" values="10; 5; 10" begin="0.15s" dur="0.6s"
                       repeatCount="indefinite"/>
            </rect>
            <rect x="16" y="10" width="4" height="10" fill="#333" opacity="0.2">
              <animate attributeName="opacity" attributeType="XML" values="0.2; 1; .2" begin="0.3s" dur="0.6s"
                       repeatCount="indefinite"/>
              <animate attributeName="height" attributeType="XML" values="10; 20; 10" begin="0.3s" dur="0.6s"
                       repeatCount="indefinite"/>
              <animate attributeName="y" attributeType="XML" values="10; 5; 10" begin="0.3s" dur="0.6s"
                       repeatCount="indefinite"/>
            </rect>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import router from "@/router";
import {io} from "socket.io-client";
import {ref} from 'vue';
import {useGameEngine} from "@/composables/useGameEngine";
import GameFormat from "@/view-models/gameFormat";

const {socket, setSocket} = useGameEngine();
const clickedStart = ref(false);
const hasStarted = ref(false);

function startGame() {
  clickedStart.value = true;
  setTimeout(() => {
    if (!hasStarted.value) {
      alert("Looks like there's a problem connecting you to the server 😕");
    }
  }, 6000);
  registerSocket();
}

function registerSocket() {
  const newSocket = io(process.env.VUE_APP_SERVER);
  setSocket(newSocket);
  socket.value.on("room", (roomId: string) => {
    hasStarted.value = true;
    router.push({path: `/game/${roomId}`});
  });
  socket.value.on("gameTypes", (gameTypes: GameFormat[]) => {
    localStorage.setItem("gameTypes", JSON.stringify(gameTypes));
  });
}
</script>

<style scoped lang="scss">
.home {
  display: flex;
  justify-content: center;
  // height: 100%;
  width: 100%;
  box-sizing: border-box;
}


.container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 800px;
}

.start-game {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 80%;
  position: relative;
}

.button {
  user-select: none;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  width: 320px;
  height: 80px;
  background: #182540;
  border-radius: 32px;
  text-align: center;
  border: none;
  cursor: pointer;
  transition: all 0.1s ease-in-out;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2),
    0 0 10px rgba(24, 37, 64, 0.8);
  color: #F0F2F2;

  &:hover {
    opacity: 0.6;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2),
                0 0 10px rgba(24, 37, 64, 0.8);
  }

  &:active {
    opacity: 1;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2),
                0 0 10px rgba(24, 37, 64, 0.8);
  }

  span {
    font-family: "Montserrat", sans-serif;
    font-size: 26px;
    font-weight: semibold;
  }
}

.disabled {
  opacity: 1;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2),
                0 0 10px rgba(24, 37, 64, 0.8);

  &:hover {
    opacity: 1;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2),
                0 0 10px rgba(24, 37, 64, 0.8);
  }
}

svg rect {
  fill: #54e8dd;
}
  .robson-image {
    // max-width: 100px;
    width: 40%;
    margin-bottom: 20px;
    margin-top: 50px;
  }

.free-poker-header {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  height: 80%;
  width: 100%;



  h1 {
    user-select: none;
    font-size: 3.2em;

    span {
      color:  #F0F2F2;
      background: #182540;
      border-radius: 10px;
      width: 11rem;
      display: inline-block;
    }
  }
}


@media only screen and (max-width: 800px) {

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 800px;
  }

  .free-poker-header {
    width: 90%;
    align-items: flex-end;
  }

  .start-game {
    align-items: flex-start;
  }
}

</style>