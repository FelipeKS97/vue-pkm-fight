<template>
  <div id="app">
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">YOU</h1>
            <div class="healthbar">
                <div 
                  class="healthbar text-center" 
                  style="background-color: green; margin: 0; color: white;"
                  :style="{width: playerHealth + '%'}"
                >
                  {{ playerHealth }}
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbar">
                <div 
                  class="healthbar text-center" 
                  style="background-color: green; margin: 0; color: white;"
                  :style="{width: enemyHealth + '%'}"
                >
                 {{ enemyHealth }}
                </div>
            </div>
        </div>
    </section>
    <section v-if="!gameIsRunning" class="row controls">
        <div class="small-12 columns">
            <button @click="gameStart" class="button" id="start-game">START NEW GAME</button>
        </div>
    </section>
    <section v-else class="row controls">
        <div class="small-12 columns">
            <button @click="attack" class="button" id="attack">ATTACK</button>
            <button @click="spAttack" class="button" id="special-attack">SPECIAL ATTACK</button>
            <button @click="heal" class="button" id="heal">HEAL</button>
            <button @click="giveUp" class="button" id="give-up">GIVE UP</button>
        </div>
    </section>
    <section class="row log">
        <div class="small-12 columns">
            <ul>
                <li>

                </li>
            </ul>
        </div>
    </section>
  </div>
</template>

<script>

export default {
  name: "app",
  data: function() { 
    return {
      playerHealth: 100,
      enemyHealth: 100,
      gameIsRunning: false
    }
  },
  methods: {
    gameStart: function() {
      this.gameIsRunning = true
      this.playerHealth = 100
      this.enemyHealth = 100
    },
    attack: function() {
      let max = 10
      let min = 3
      let damage = Math.floor(Math.random()*(max-min+1)+min);
      //damage = Math.max(Math.floor(Math.random * max) + 1, min)
      console.log(damage)
      this.enemyHealth -= damage;

      max = 12
      min = 5
      damage = Math.floor(Math.random()*(max-min+1)+min);
      this.playerHealth -= damage;
    },
    spAttack: function() {
      
    },
    heal: function() {
      
    },
    giveUp: function() {
      this.gameIsRunning = false
      this.playerHealth = 100
      this.enemyHealth = 100
    },
  },
};
</script>

<style>
 @import '../css/app.css';
 @import '../css/foundation.min.css';
</style>
