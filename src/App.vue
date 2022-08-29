<template>
  <main-screen 
    v-if="statusMatch ==='default'" 
    @onStart='onHandleBeforeStart($event)'
  />  
  <interact-screen 
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"    
  />
  <result-screen 
    v-if="statusMatch==='result'"
    :timer="timer"
    @onStartAgain="statusMatch='default'"
  />
</template>

<script>

import MainScreen from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
import ResultScreen from './components/ResultScreen.vue';

import {shuffled} from './utils/array'

export default {
  name: 'App',
  data(){
    return {
      settings:{
        totalOfBlocks:0,
        cardsContext:[],
        startedAt:null
      },
      statusMatch:"default",
      timer:0
    }
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen
  },
  methods:{
    onHandleBeforeStart(config){
      this.settings.totalOfBlocks = config.totalOfBlocks
      
      const firstCards = Array.from({length:this.settings.totalOfBlocks/2},(_,i)=>i+1)
      const secondCards = [...firstCards, ...firstCards]
      this.settings.cardsContext = shuffled(shuffled(shuffled(secondCards)))
      
      this.settings.startedAt = new Date().getTime()

      //data ready
      this.statusMatch = 'match'
    },
    onGetResult(){
      //Get time
      this.timer = new Date().getTime() - this.settings.startedAt

      //switch to result
      this.statusMatch = 'result'
    }
  }
}
</script>
