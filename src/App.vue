<template>

  <nav>
    <div class="container">
      <div class="nav-wrapper">
        <a href="/">AOS Tracker</a>
        <ul class="right">
          <li><a href="/" v-on:click.prevent.stop="refresh()" title="Clear data"><i class="material-icons">refresh</i></a></li>
        </ul>
      </div>
    </div>
  </nav>

  <div class="container" id="content" >
    <PlayerSetup v-if="!players" @savePlayers="onSavePlayers" />
    
    <div v-if="players">
      <div class="row">
        <div class="col s12 m6 l6" v-for="(player, index) in players" :key="index">
          <h3>{{player.name}} - {{calcFinalScore(index)}} points</h3>
          <p><strong>Grand strategy:</strong> {{player.strat}} 
            <span class="switch gs-switch">
              <label>No<input type="checkbox" v-model="players[index].grandStategy"><span class="lever"></span>Yes (3 VPs)</label>
            </span>
          </p>

          <div class="round-grid" v-for="(round, roundIndex) in player.rounds" :key="roundIndex">
            <div class="round-grid-title">
              <div>Round {{roundIndex + 1}}</div>
              <div>{{roundScore(index, roundIndex)}} VPs</div>
            </div>
            <div class="round-grid-body">
              <div class="input-field col s12">
                <input :id="'obj'+roundIndex" type="text" v-model="players[index].rounds[roundIndex].tactic">
                <label class="" :for="'obj'+roundIndex">Objective</label>
              </div>
              <div class="round-grid-points-container">
                <div class="round-grid-points-container-items" >
                  <span>Objective Points</span>
                  <div>
                    <button class="btn-floating btn-small waves-effect waves-light red" @click="decOP(index, roundIndex)"><i class="material-icons right">exposure_neg_1</i></button>
                    {{players[index].rounds[roundIndex].objectivePoints}}
                    <button class="btn-floating btn-small waves-effect waves-light red" @click="incOP(index, roundIndex)"><i class="material-icons right">exposure_plus_1</i></button>
                  </div>
                  <span>Monster Kill</span>
                  <div class="switch">
                    <label>No<input type="checkbox" v-model="players[index].rounds[roundIndex].monsterKill"><span class="lever"></span>Yes (1 VPs)</label>
                  </div>
                </div>
                <div class="round-grid-points-container-items" >
                  <span>Battle tactic</span>
                  <div class="switch">
                    <label>No<input type="checkbox" v-model="players[index].rounds[roundIndex].battleTactic"><span class="lever"></span>Yes (2 VPs)</label>
                  </div>
                  <span>Battle tactic monster bonus</span>
                  <div class="switch">
                    <label>No<input type="checkbox" v-model="players[index].rounds[roundIndex].battleTacticMonsterBonus"><span class="lever"></span>Yes (1 VPs)</label>
                  </div>
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>
    </div>

  </div>

</template>

<script>
import PlayerSetup from './components/PlayerSetup.vue'

export default {
  name: 'App',
  components: {
    PlayerSetup
  },
  data: function () {
    return {
      players: null
    }
  },
  mounted() {
    if(localStorage.players) this.players = JSON.parse(localStorage.players)
  },
  watch:{
    players: {
      handler: function(newPlayers) {
        localStorage.players = JSON.stringify(newPlayers)
      },
      deep: true
    }
  },
  methods: {
    onSavePlayers(value) {
      this.players = value
    },
    incOP(player, round) {
      this.players[player].rounds[round].objectivePoints ++
    },
    decOP(player, round) {
      this.players[player].rounds[round].objectivePoints --
    },
    refresh() {
      localStorage.clear()
      this.players = null
    },
    roundScore(player, round) {
      let objPoints = this.players[player].rounds[round].objectivePoints;
      let monsterKill = 0
      let battleTactic = 0
      let battleTacticMonsterBonus = 0
      
      if(this.players[player].rounds[round].monsterKill) {
        monsterKill = 1
      }
      
      if(this.players[player].rounds[round].battleTactic) {
        battleTactic = 2
      }
      
      if(this.players[player].rounds[round].battleTacticMonsterBonus) {
        battleTacticMonsterBonus = 1
      }
      return objPoints + monsterKill + battleTactic + battleTacticMonsterBonus
    },
    calcFinalScore(player) {
      let total = 0;
      this.players[player].rounds.forEach(element => {
        let objPoints = element.objectivePoints;
        let monsterKill = 0
        let battleTactic = 0
        let battleTacticMonsterBonus = 0
        
        if(element.monsterKill) {
          monsterKill = 1
        }
        
        if(element.battleTactic) {
          battleTactic = 2
        }
        
        if(element.battleTacticMonsterBonus) {
          battleTacticMonsterBonus = 1
        }
        let subtotal = objPoints + monsterKill + battleTactic + battleTacticMonsterBonus
        total = total + subtotal
      });

      if(this.players[player].grandStategy) {
        total = total + 3
      }
     
      return total
    }
  }
}
</script>


<style scoped>
  #content {
    margin-top:40px;
  }
  .card-grid {
    display:grid;
    grid-template-columns: 80px auto 80px;
    grid-column-gap: 10px;
  }
  .card-grid-points {
    text-align: center;
    border-right:1px solid #ddd
  }
  .card-grid-objective {
  }
  .card-grid-bt {
    text-align: center;
    border-left:1px solid #ddd
  }

  .round-grid {
    display:grid;
    box-shadow: 0 2px 2px 0 rgb(0 0 0 / 14%), 0 3px 1px -2px rgb(0 0 0 / 12%), 0 1px 5px 0 rgb(0 0 0 / 20%);
    border-radius: 2px;
    margin-bottom:10px;
    grid-template-rows: auto auto;
    padding:10px;
  }

  .round-grid-title {
    border-bottom:1px solid #ddd;
    padding:10px;
    display:flex;
    justify-content: space-between
  }
  .round-grid-body {
    display:grid;
    grid-template-rows: auto auto;
  }
  .round-grid-points-container {
    display:grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 10px;
  }
  .round-grid-points-container-items {
    display:flex;
    flex-direction:column;
    text-align:center
  }

  .gs-switch {
    float:right;
  }
</style>
