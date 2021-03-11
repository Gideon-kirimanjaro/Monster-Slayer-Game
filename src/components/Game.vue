<template>
    <v-main>
        <v-container style="width: 30%; margin-top: 200px" class="text-align-center">
            <div style="height: 15%; " class="brown white--text">
            <h3  class="ml-5 pl-5 display-1" >MONSTER SLAYER</h3>
            </div>
            <div>
                <h4>Monster Health</h4>
                <v-btn :style="widthMonster"  v-model="monsterHealth" class="white--text" color="blue"></v-btn>
            </div>
            <div>
                <h4>Your Health</h4>
                <v-btn :style="widthPlayer"  v-model="playerHealth" class="white--text" color="blue"></v-btn>
            </div>
            <div v-if="winner">
                <h3>Game over</h3>
                <h3 v-if="winner === 'player'">YOU WON</h3>
                <h3 v-if="winner ==='monster'">YOU LOST</h3>
                <h3 v-if="winner=== 'draw'">YOU DREW</h3>
            </div>
            <div class="mt-5 d-flex flex-column">
                <v-btn @click="attackMonster()" depressed   class=" white--text mb-3 pa-2 " color="pink" >Attack</v-btn>
                <v-btn :disabled="counterLimit" @click="specialAttackMonster()" depressed  class=" white--text mb-3 pa-2 " color="pink" >Special Attack</v-btn>
                <v-btn @click="healPlayer()"  depressed class=" white--text mb-3 pa-2 " color="pink" >Heal</v-btn>
            </div>

        </v-container>
    </v-main>

    
</template>

<script>
function randomNumber(max,min){
    return Math.floor(Math.random()*(max-min)+ min)
}
    export default {
        name: 'Game',
        watch:{
            playerHealth(value){
                if(value<=0 && this.monsterHealth<=0)
                {
                    this.winner = 'draw';
                } else if(value<=0){
                    this.winner = 'monster'

                }
            },
            monsterHealth(value){
                if(value<=0 && this.playerHealth<=0)
                {
                    this.winner = 'draw'
                } else if(value<=0){
                    this.winner = 'player'

                }
            },

        },
        computed:{
            counterLimit(){
                return this.counter % 3==0
            },
            widthMonster(){
                if(this.monsterHealth<0){
                    return {width: '0%'}
                }
                return {width: this.monsterHealth + '%' }
            },
            widthPlayer(){
                return {width: this.playerHealth + '%' }
            }

        },
        methods:{
           healPlayer(){
                const healValue = randomNumber(20,8);
                if(this.playerHealth+healValue>100){
                    return this.playerHealth == 100
                }else {
                    return this.playerHealth+=healValue
                }
                
            },
            attackPlayer(){
                const attackValue2 = randomNumber(15,8);
                this.playerHealth= this.playerHealth - attackValue2;
            },
            attackMonster(){
                this.counter++;
                const attackValue = randomNumber(12,5);
                this.monsterHealth-=attackValue;
                this.attackPlayer();
                
            },
            specialAttackMonster(){
               const attackValue = randomNumber(10,20);
               this.monsterHealth = this.monsterHealth-attackValue;
               this.attackPlayer()
            }
            
        }
        ,
        data(){
            return{
                playerHealth: 100,
                monsterHealth: 100,
                counter: 0,
                winner: null,
            }
        }
    }
</script>

<style>
.active{
    background-color: blue;
    background-size: 100% ;
}

</style>