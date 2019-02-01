<template>
  <div id="app">

    <section class="row text-center">
      <img class="logo" :src="pokevue" />
    </section>
    <section class="row">
        <div class="small-6 columns">
            <div class="pokemon-section">
              <img id="player-pokemon" :src="playerPokemon.url" />
            </div>
            <h1 class="text-center">YOU {{playerPokemon.name}}</h1>
            <div class="healthbar">
              <div class="main-selection">
                <select v-if="!gameIsRunning" id="player" @change="selectPokemon">
                  <option selected value=""></option>
                  <option 
                    :key="pkm.url" 
                    v-for="pkm in pkmData" 
                    :value="pkm.url"
                  >
                    {{pkm.name}}
                  </option>
                </select>
              </div>
              <div 
                class="healthbar text-center" 
                style="background-color: #41b883; margin: 0; color: white;"
                :style="{width: playerHealth + '%'}"
              >
                {{ playerHealth }}
              </div>
            </div>
        </div>
        <div class="small-6 columns">
            <div class="pokemon-section">
              <img :src="enemyPokemon.url" />
            </div>
            <h1 class="text-center">ENEMY {{enemyPokemon.name}}  </h1>
            <div class="healthbar">
              <div class="main-selection">
                <select v-if="!gameIsRunning" id="enemy" @change="selectPokemon">
                  <option selected value=""></option>
                  <option
                    :key="pkm.url"
                    v-for="pkm in pkmData" 
                    :value="pkm.url"
                  >
                    {{pkm.name}}
                  </option>
                </select>
              </div>
                <div 
                  class="healthbar text-center" 
                  style="background-color: #41b883; margin: 0; color: white;"
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
    <section class="row log" v-if="turns.length > 0">
        <div class="small-12 columns">
            <ul>
                <li 
                  v-for="turn in turns"
                  :class="{'player-turn': turn.isPlayer, 'enemy-turn': !turn.isPlayer}"
                >
                  {{turn.text}}
                </li>
            </ul>
        </div>
    </section>
  </div>
</template>

<script>

import pokevue from './assets/pokevue.png'

export default {
  name: "app",
  data: function() { 
    return {
      playerHealth: 100,
      enemyHealth: 100,
      playerPokemon: {
        name: '',
        url:''
      },
      enemyPokemon: {
        name: '',
        url:''
      },
      gameIsRunning: false,
      turns: [],
      pkmData: [
        {name:'Tyranitar', url: 'https://cdn.discordapp.com/emojis/396699704118738955.gif?v=1'},
        {name:'Snorlax', url: 'https://cdn.discordapp.com/emojis/396695217019027456.gif?v=1'},
        {name:'Dragonite', url:'https://cdn.discordapp.com/emojis/396695210895081472.gif?v=1'},
        {name:'Hariyama', url: 'https://cdn.discordapp.com/emojis/396700081857757184.gif?v=1'},
        {name:'Pinsir', url: 'https://cdn.discordapp.com/emojis/396695206331940864.gif?v=1'},
        {name:'Gyarados', url:' https://cdn.discordapp.com/emojis/396695217128079360.gif?v=1'},
        {name:'Arcanine', url: 'https://cdn.discordapp.com/emojis/396694941340008448.gif?v=1'},
        {name:'Rhydon', url: 'https://cdn.discordapp.com/emojis/396695213017399296.gif?v=1'},
        {name:'Lapras', url: 'https://cdn.discordapp.com/emojis/396695214905098241.gif?v=1'},
        {name:'Blastoise', url: 'https://cdn.discordapp.com/emojis/396694677157576704.gif?v=1'},
        {name:'Charizard', url: 'https://cdn.discordapp.com/emojis/403110375534493696.gif?v=1'},
        {name:'Venusaur', url: 'https://cdn.discordapp.com/emojis/396694675735445505.gif?v=1'},
        {name:'Ursaring', url: 'https://cdn.discordapp.com/emojis/396699699320717312.gif?v=1'},
        {name:'Blissey', url:'https://cdn.discordapp.com/emojis/396699701254160384.gif?v=1'}
      ],
      pokevue: pokevue
    }
  },
  methods: {
    gameStart: function() {
      if(this.playerPokemon.url === '' || this.enemyPokemon.url === '' ) {
        alert('Select Pokemons for the battle!')
      } else {
        this.gameIsRunning = true
        this.playerHealth = 100
        this.enemyHealth = 100
        this.turns = []
      }
    },
    attack: function() {

      let damage = this.calculateDamage(4,11);

      this.enemyHealth -= damage

      this.turns.unshift({
        isPlayer: true,
        text: 'Player Pokemon hits Enemy for' + damage + 'points!'
      })

      if(this.checkWin()) {
        return;
      }
       this.enemyAttack(5,12);
    },
    spAttack: function() {
      let damage = this.calculateDamage(10,20);
      this.enemyHealth -= damage
      this.turns.unshift({
        isPlayer: true,
        text: 'Player Pokemon hits SP on Enemy for' + damage + 'points!'
      })
      if(this.checkWin()) {
        return;
      }

      this.enemyAttack(10,20);
    },
    heal: function() {
      if(this.playerHealth <= 90) {
        this.playerHealth += 10
      } else {
        this.playerHealth = 100
      }

      this.turns.unshift({
        isPlayer: true,
        text: 'Player Pokemon heals for 10 points!'
      })
      
      this.enemyAttack(6,13)
    },
    giveUp: function() {
      this.gameIsRunning = false
      this.turns = []
      this.playerHealth = 100
      this.enemyHealth = 100
    },
    calculateDamage: function(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    enemyAttack: function(min, max) {
      let damage = this.calculateDamage(min, max);
      this.playerHealth -= damage
      this.checkWin();
      this.turns.unshift({
        isPlayer: false,
        text: 'Enemy Pokemon hits Player for ' + damage + ' points!'
      })
    },
    checkWin: function() {
      if(this.enemyHealth <= 0 ) {
        if(confirm("You have won! New Game?")) {
          this.gameStart()
        } else {
          this.gameIsRunning = false
        }
        return true;
      } else if(this.playerHealth <= 0) {
          if(confirm("You've lost! New Game?")) {
            this.gameStart()
          } else {
            gameIsRunning = false
          }
        return true;
      }

      return false;

    },
    selectPokemon: function(event) {
      if(event.target.id === 'player') {
        this.playerPokemon.url = event.target.value
      } else {
        this.enemyPokemon.url = event.target.value   
      }
    }
  },
};
</script>

<style>
 @import '../css/app.css';
 @import '../css/foundation.min.css';
</style>
