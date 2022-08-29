<template>
  <div class="screen">
    <div class="screen__inner"
    :style="{
        width: `${
          ((((700 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip 
        v-for="(card, index) in cardsContext" 
        :key='index'
        ref="cards"
        :card="{index, value:card}"
        :cardsContext="cardsContext"
        :imgBackFaceUrl="`images/${card}.png`"
        @onFlip="checkRule($event)"
      />
    </div>
    </div>
</template>

<script>
  import CardFlip from './Card.vue'

export default {
  props:{
    cardsContext:{
      type:Array,
      default: function (){
        return []
      }
    }
  },
  components: {
    CardFlip
  },
  data(){
    return{
      rules:[],
      disabledElements:0
    }
  },
  methods:{
    checkRule(card){
        if(this.rules.length===2) return false
        console.log(this.rules);

        this.rules.push(card)

        if(this.rules.length===2 && this.rules[0].value===this.rules[1].value){
            //add class disabled
            this.$refs.cards[this.rules[0].index].onEnabledDisable()
            this.$refs.cards[this.rules[1].index].onEnabledDisable()
            //clear rules
            this.rules=[]

            this.disabledElements = this.disabledElements + 2;
            if(this.disabledElements &&this.disabledElements>=this.cardsContext.length - 2){
              setTimeout(()=>{
                this.$emit('onFinish')
              },920)
            }
        }else if (this.rules.length===2){
          setTimeout(()=>{
            //close two cards
            this.$refs.cards[this.rules[0].index].onFlipBackCard()
            this.$refs.cards[this.rules[1].index].onFlipBackCard()
            // clear rules
            this.rules=[]
          },800)
        }else return false
    }
  }
}
</script>

<style scoped>
  .screen{
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index:2;
    background-color: var(--dark);
    color:var(--light)
  }
  .screen__inner{
    width: 424px;
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
  }
</style>