<template>
  <div>
    <!-- Display Jogador vs Montro -->
    <div class="hpContainer">
      <div class="playerBox">
        <h1>Jogador 1</h1>
        <div class="hp">{{ playerLife + "%" }}</div>
        <div class="lifeBar">
          <div
            class="life"
            :class="{ danger: playerLife < 20 }"
            :style="{ width: playerLife + '%' }"
          ></div>
        </div>
      </div>
      <div class="playerBox">
        <h1 style="color: red">Monstro</h1>
        <div class="hp">{{ monsterLife + "%" }}</div>
        <div class="lifeBar">
          <div
            class="life"
            :class="{ danger: monsterLife < 20 }"
            :style="{ width: monsterLife + '%' }"
          ></div>
        </div>
      </div>
    </div>

    <!-- Win/Lose -->
    <div class="winContainer" v-show="winnerOrLose()">
      <p
        v-if="monsterLife == 0"
        style="margin: auto auto; font-size: 200%; color: green"
      >
        Você Ganhou!!!
      </p>
      <p v-else style="margin: auto auto; font-size: 200%; color: red">
        Você Perdeu!!!
      </p>
    </div>

    <!-- Botões <start> <Atk> <Atkspecial> <Curar> <Desistir> -->

    <div v-show="containerOn" class="btnContainer">
      <button @click="startGame()" class="start">
        <strong>Começar Jogo</strong>
      </button>
    </div>
    <div v-show="!containerOn" class="btnContainer">
      <button @click="atk(false)" class="atk"><strong>Atacar</strong></button>
      <button @click="atk(true)" class="atkEspecial">
        <strong>Ataque Especial</strong>
      </button>
      <button @click="healAndHurt()" class="curar">
        <strong>Curar</strong>
      </button>
      <button @click="desistir()" class="desistir">
        <strong>Desistir</strong>
      </button>
    </div>

    <!-- Lista de Logs -->
    <div v-if="logs.length" class="logContainer">
      <ul>
        <li v-for="log in logs" :class="log.cls" class="log" :key="log">
          {{ log.text }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      containerOn: true,
      playerLife: 100,
      monsterLife: 100,
      logs: [],
    };
  },

  computed: {},

  methods: {
    startGame() {
      this.containerOn = !true;
      this.playerLife = 100;
      this.monsterLife = 100;
      this.logs = [];
    },
    winnerOrLose() {
      return this.playerLife == 0 || this.monsterLife == 0;
    },
    desistir() {
      this.containerOn = !false;
      this.playerLife = 100;
      this.monsterLife = 100;
      this.logs = [];
    },
    atk(especial) {
      this.hurt("playerLife", 7, 12, false, "Monstro", "Jogador", "monster");
      if (this.monsterLife > 0) {
        this.hurt("monsterLife", 7, 12, especial, "Jogador", "Monstro", "player")
      
      }
    },
    hurt(prop, min, max, especial, source, target, cls) {
      const plus = especial ? 5 : 0;
      const hurt = this.getRandom(min + plus, max + plus);
      this[prop] = Math.max(this[prop] - hurt, 0);
      this.registerLog(`${source} atingiu ${target} com ${hurt}`, cls);
    },
    heal(min, max) {
      const heal = this.getRandom(min, max);
      this.playerLife = Math.min(this.playerLife + heal, 100);
      this.registerLog(`Jogador ganhou força de ${heal}`, "player");
    },
    healAndHurt() {
      this.heal(10, 15);
      this.hurt("playerLife", 7, 12, false, "Monstro", "Jogador", "monster");
    },
    getRandom(min, max) {
      const value = Math.random() * (max - min) + min;
      return Math.round(value);
    },
    registerLog(text, cls) {
      this.logs.unshift({ text, cls });
    },
  },

  watch: {},
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  text-align: center;
  font-family: helvetica;
  color: green;
  font-size: 30px;
}

.hpContainer {
  width: 80%;
  margin: auto auto auto auto;
  background-color: whitesmoke;
  height: 300px;
  border-style: solid;
  border-color: transparent;
  box-shadow: 5px 5px 5px 5px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  align-items: center;
}

.playerBox {
  width: 45%;
  height: 50%;
}

.hp {
  text-align: center;
}

.lifeBar {
  width: 100%;
  height: 20px;
  border: 1px solid #aaa;
  margin-top: 10px;
}

.lifeBar .life {
  display: flex;
  justify-content: center;
  height: 100%;
  width: 100%;
  background-color: green;
}

.lifeBar .life.danger {
  background-color: red;
}

.winContainer {
  width: 80%;
  margin: 20px auto auto auto;
  background-color: whitesmoke;
  height: 100px;
  border-style: solid;
  border-color: transparent;
  box-shadow: 5px 5px 5px 5px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

.btnContainer {
  width: 80%;
  margin: 20px auto auto auto;
  background-color: whitesmoke;
  height: 100px;
  border-style: solid;
  border-color: transparent;
  box-shadow: 5px 5px 5px 5px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

button {
  width: 150px;
  height: 50px;
  margin: auto auto;
  font: Helvetica;
  border-radius: 5px;
  border-color: unset;
}

.start {
  background-color: blue;
  color: white;
  width: 300px;
  height: 50px;
}
.atk {
  background-color: red;
  color: white;
}
.atkEspecial {
  background-color: orangered;
}

.curar {
  background-color: green;
  color: white;
}

.desistir {
  background-color: gray;
}

.logContainer {
 width: 80%;
  margin: 20px auto auto auto;
  background-color: whitesmoke;
  border-style: solid;
  border-color: transparent;
  box-shadow: 5px 5px 5px 5px rgba(0, 0, 0, 0.2);
  
}

/* Área dos Logs */

.logs ul {
  display: flex;
  flex-direction: column;
  list-style: none;
  padding: 0;
  margin: 0;
}

.logs ul li {
  display: flex;
  justify-content: center;
  margin: 4px 0px;
  padding: 3px 0px;
  font-weight: 600;
  font-size: 1.1rem;
  text-transform: uppercase;
  border-radius: 3px;
}

.player {
  background-color: #4253afaa;
  color: #fff;
}

.monster {
  background-color: #e51c23aa;
  color: #fff;
}
</style>
