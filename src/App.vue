<template>
  <div id="app" class="container-fluid">
    <div class="row justify-content-center">
      <div class="col-lg-3 col-md-3 col-sm-3">
      <h2>Player</h2>
      <div class="progress">
        <div class="progress-bar bg-success progress-bar-striped progress-bar-animated" role="progressbar" :style="{width: player+'%'}" aria-valuenow="player" aria-valuemin="0" aria-valuemax="100">{{player}}%</div>
      </div>
    </div>
    <div class="col-lg-3 col-md-3 col-sm-3">
      <h2>Monster</h2>
      <div class="progress">
        <div class="progress-bar bg-danger progress-bar-striped progress-bar-animated" role="progressbar" :style="{width: monster+'%'}" aria-valuenow="player" aria-valuemin="0" aria-valuemax="100">{{monster}}%</div>
      </div>
    </div>
    </div>
    <div class="p-4">
      <div v-if="!startGame">
        <button class="btn btn-primary" @click="startGame = true">New Game</button>
      </div>
      <div v-else>
        <button class='btn btn-primary m-1' @click="attack">Attack</button>
      <button class='btn btn-primary m-1' @click="sAttack">Special Attack</button>
      <button class='btn btn-primary m-1' @click="heal">Heal</button>
      <button class='btn btn-primary m-1' @click="giveUp">Give up</button>
      </div>
    </div>
    <div class="row justify-content-center">
      <div id="logs" class="col-lg-6 col-md-6">
        <div :class="{player:value.who=='Player',monster:value.who=='Monster'}" :key="index" v-for="(value,index) in attacks">{{value.who}} did a {{value.what}} of {{value.howMuch}}</div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      player:100,
      monster:100,
      startGame: false,
      attacks:[]
    }
  },
  components: {
  },
  methods:{
    attack(){
      var randomPlayer =  this.random(0,10)
      this.attacks.push({
        who : 'Player',
        what : 'attack',
        howMuch : randomPlayer
      });
      var randomMonster = this.random(0,10)
      this.attacks.push({
        who : 'Monster',
        what : 'attack',
        howMuch : randomMonster
      });
      this.player -= randomPlayer;
      this.monster -= randomMonster;
      console.log(this.attacks)
    },
    sAttack(){
      var randomPlayer =  this.random(10,20)
      var randomMonster = this.random(10,20)
      this.attacks.push({
        who : 'Player',
        what : 'special attack',
        howMuch : randomPlayer
      });
      this.attacks.push({
        who : 'Monster',
        what : 'special attack',
        howMuch : randomMonster
      });
      this.player -= randomPlayer;
      this.monster -= randomMonster;
    },
    heal(){
      var randomPlayer =  this.random(1,3)
      var randomMonster = this.random(1,3)
      this.attacks.push({
        who : 'Player',
        what : 'heal',
        howMuch : randomPlayer
      });
      this.attacks.push({
        who : 'Monster',
        what : 'heal',
        howMuch : randomMonster
      });
      this.player += randomPlayer;
      this.monster += randomMonster;
    },
    giveUp(){
      var losing = confirm("Do you wanna give up ?");
      this.attacks.push({
        who : 'Player',
        what : 'give up',
        howMuch : 'game'
      });
      if(losing){
        this.player = 0;
      }
    },
    random(min,max){
      var down = Math.ceil(min);
      var up = Math.floor(max);
      return Math.floor(Math.random() * (up - down)) + down
    }
  },
  watch:{
    player(){
      if(this.player<=0){
        alert('Lost :(');
        var restart = confirm('Revenge Time : Start New Game ?');
        if(restart){
          location.reload();
        }
        
      }
    },
    monster(){
      if(this.monster<=0){
        alert('Won !');
        var restart = confirm('Party Time : Start New Game ?');
        if(restart){
          location.reload();
        }
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#logs{
  height: 50vh;
  overflow-y: auto;
}

.monster{
  background-color: lightsalmon;
  color: red;  
}

.player{
  background-color: lightgreen;
  color: green;
}

#progress{
    display: flex;
  justify-content: space-evenly;
}

#pProg,#mProg{
  text-align: center;
}
</style>
