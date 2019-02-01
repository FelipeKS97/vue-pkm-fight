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
    <section class="row controls">
        <div class="small-12 columns">
            <button class="button" id="start-game">START NEW GAME</button>
        </div>
    </section>
    <section class="row controls">
        <div class="small-12 columns">
            <button class="button" id="attack">ATTACK</button>
            <button class="button" id="special-attack">SPECIAL ATTACK</button>
            <button class="button" id="heal">HEAL</button>
            <button class="button" id="give-up">GIVE UP</button>
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
  }
};
</script>

<style>
 @import '../css/app.css';
 @import '../css/foundation.min.css';
</style>
