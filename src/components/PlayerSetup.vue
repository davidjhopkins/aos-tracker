<template>

  <div class="row">
    <div class="col s12 m6 l4 offset-l1">
      <div class="row">
        <div class="col s12"><h5>Player One</h5></div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input  id="playerOneName" type="text" v-model="players[0].name">
          <label class="" for="playerOneName">Name</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input  id="playerOneGrandStrat" type="text" v-model="players[0].strat">
          <label class="" for="playerOneGrandStrat">Grand Strategy</label>
        </div>
      </div>
    </div>

    <div class="col s12 m6 l4 offset-l1">
      <div class="row">
        <div class="col s12"><h5>Player Two</h5></div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input  id="playerTwoName" type="text" v-model="players[1].name">
          <label class="" for="playerTwoName">Name</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input  id="playerTwoGrandStrat" type="text" v-model="players[1].strat">
          <label class="" for="playerTwoGrandStrat">Grand Strategy</label>
        </div>
      </div>
    </div>
  </div>

  <div class="row">
    <div class="col s12 center-align">
      <button class="btn waves-effect waves-light" name="action" :disabled="!isDisable" @click="savePlayers">Save</button>
    </div>
  </div>

</template>

<script>
export default {
  name: 'PlayerSetup',
  emits: ['savePlayers'],
  data: function () {
    return {
      players: [
        {
          name: '',
          strat: '',
          points: 0,
          grandStategy: false,
          rounds: [
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            }
          ]
        },
        {
          name: '',
          strat: '',
          points: 0,
          grandStategy: false,
          rounds: [
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            },
            {
              vps: 0,
              tactic: '',
              objectivePoints: 0,
              monsterKill: false,
              battleTactic: false,
              battleTacticMonsterBonus: false
            }
          ]
        }
      ]
    }
  },
  props: {
    msg: String
  },
  computed: {
    isDisable() {
     return  this.players[0].name !== '' && this.players[0].strat !== ''
    }
  },
  methods: {
    savePlayers() {
      if(this.players[1].name !== '') {
        this.$emit('savePlayers', this.players)
      } else {
        let tempArray = []
        tempArray.push(this.players[0])
        this.$emit('savePlayers', tempArray)
      }
    }
  }
}
</script>
