<template>
  <div class="hello">
    <div class="panel scores">
      <div class="score">
        <h2>Jogador</h2>
        <div class="lifeBar">
          <div class="life" :class="{danger: playerLife < 20}" :style="{width: playerLife + '%'}"></div>
        </div>
        <h3>{{playerLife}}</h3>
      </div>
      <div class="score">
        <h2>Monstro</h2>
        <div class="lifeBar">
          <div class="life" :class="{danger: monsterLife < 20}" :style="{width: monsterLife + '%'}"></div>
        </div>
        <h3>{{monsterLife}}</h3>
      </div>
    </div>
      
    <div  v-if="hasResult" class="panel result">
      <div v-show="this.monsterLife == 0" class="win">Voce ganhou!</div>
      <div v-show="this.playerLife == 0" class="loser">Voce perdeu" :(</div>
    </div>

    <div class="panel buttons">
      <template v-if="running">
        <button class="btn attack" @click="attack(false)">Ataque</button>
        <button class="btn especial-attack " @click="attack(true)">Ataque Especial</button>
        <button class="btn heal" @click="healAndHurt()">Curar</button>
        <button class="btn give" @click="running = false">Desistir</button>
      </template>
        <button v-else class="btn new" @click="start">Iniciar jogo</button>
        
    </div>

    <div v-if="logs.length" class="panel logs">
        <ul>
          <li v-for="log in logs" :key="log.lenght"
          :class="log.cls">{{log.text}}</li>
        </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return {
      running: false,
      playerLife:'100',
      monsterLife:'100',
      backgroundColor:'green',
      logs:[]
    }
  },
  computed:{
    hasResult(){
      return this.playerLife == 0 || this.monsterLife == 0
    }

  },
  methods: {
    start(){
      this.running = true
      this.playerLife = 100
      this.monsterLife = 100
      this.logs = []
    },
    attack(especial){
      this.hurt('monsterLife', 5, 10, especial, 'Jogador', 'Monstro', 'player')
      if(this.monsterLife > 0){
        this.hurt('playerLife', 7, 12, false, 'Monstro', 'Jogador', 'monster')
        }
    },
    hurt(atr, min, max, especial, source, target, cls ){
      const plus = especial ? 5 : 0
      const hurt = this.getRandom(min + plus, max + plus)
      this[atr] = Math.max(this[atr] - hurt, 0)
      this.registerLog(`${source} atingiu ${target} com ${hurt}.`, cls)
    },
    healAndHurt(){
      this.heal(10, 15)
      this.hurt('playerLife', 7, 12, false, 'Monstro', 'Jogador', 'monster')
    },
    heal(min,max){
      const heal = this.getRandom(min, max)
      this.playerLife =Math.min(this.playerLife + heal, 100)
      this.registerLog(`Jogador ganhou força de ${heal}.`, 'player')
    },
    getRandom(min,max){
      const value = Math.random() * (max - min) + min
      return Math.round(value)
    },
    registerLog(text,cls){
      this.logs.unshift({text, cls})
    }
  },
  watch:{
    hasResult(value){
      if(value) this.running = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.panel{
  margin: 10px;
  padding: 20px;
  box-shadow: 3px 3px 5px 6px #ccc ;
}
/*  */
.scores{
  display: flex;
}
.score{
  flex:1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.lifeMonster{
  background-color: green;
  height: 100%;
}
.lifePlayer{
  background-color: green;
  height: 100%;
}
.lifeBar{
  height: 20px;
  width: 80%;
  border: 1px solid #000;
}
.lifeBar .life{
  display: flex;
  justify-content: center;
  height: 100%;
  width: 100%;
  background-color: green;
}
.lifeBar .life.danger{
  background-color: red;
}
.win{
  color: green;
  -webkit-transition: all 0.7s ease;
  transition: all 0.7s ease;
}
@-webkit-keyframes swing
{
    15%
    {
        -webkit-transform: translateX(5px);
        transform: translateX(5px);
    }
    30%
    {
        -webkit-transform: translateX(-5px);
       transform: translateX(-5px);
    } 
    50%
    {
        -webkit-transform: translateX(3px);
        transform: translateX(3px);
    }
    65%
    {
        -webkit-transform: translateX(-3px);
        transform: translateX(-3px);
    }
    80%
    {
        -webkit-transform: translateX(2px);
        transform: translateX(2px);
    }
    100%
    {
        -webkit-transform: translateX(0);
        transform: translateX(0);
    }
}
@keyframes swing
{
    15%
    {
        -webkit-transform: translateX(5px);
        transform: translateX(5px);
    }
    30%
    {
        -webkit-transform: translateX(-5px);
        transform: translateX(-5px);
    }
    50%
    {
        -webkit-transform: translateX(3px);
        transform: translateX(3px);
    }
    65%
    {
        -webkit-transform: translateX(-3px);
        transform: translateX(-3px);
    }
    80%
    {
        -webkit-transform: translateX(2px);
        transform: translateX(2px);
    }
    100%
    {
        -webkit-transform: translateX(0);
        transform: translateX(0);
    }
}
.win:hover{
  -webkit-animation: swing 1s ease;
  animation: swing 1s ease;
  -webkit-animation-iteration-count: 5;
  animation-iteration-count: 5;
  /* -webkit-transform: scale(1.6);
  transform: scale(1.6); */
}
.loser{
  color:red;
}
.result{
  display: flex;
  justify-content: center;
  font-weight: 600;
}
.bottons{
  display: flex;
  justify-content: center;
}
.btn{
  padding: 5px 10px;
  margin: 0 10px;
  border-radius: 5px;
  text-transform: uppercase;
}
.new{
  background-color: #4253af;
  color: #fff;
}
.attack{
  background-color: #e51c23;
  color: #fff;
}
.especial-attack{
  background-color: #ff9d00;
}
.heal{
  background-color: #259b24;
  color: #fff;
}
.give{
  background-color: #bbb;
}
.logs ul{
  display: flex;
  flex-direction: column;
  list-style: none;
  padding: 0;
  margin: 0;
}
.logs ul li{
  display: flex;
  justify-content: center;
  margin: 4px 0;
  padding: 3px 0;
  font-weight: 600;
  text-transform: uppercase;
  border-radius: 3px;
}
.player{
  background-color: #4253afaa;
  color: #fff;
}
.monster{
  background-color: #e51c23aa;
  color: #fff;
}
</style>
