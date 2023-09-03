<template>
    <div class="pg__indicator">
        <div v-for="stat in stats" class="pg__indicator_column">
            <div class="pg__indicator_column_sq" :class="{'pg__indicator_column_sq--green':stat[0]}"></div>
            <div class="pg__indicator_column_sq" :class="{'pg__indicator_column_sq--red':stat[1]}"></div>
        </div>
    </div>
    <div class="pg">
        <div class="pg__back">
            <img src="@/assets/back.png" alt="">
        </div>
        <div class="pg__person" :class="{'pg__person--jump': isJumping}">
            <img :src="currentPerson" alt="">
        </div>
        <p class="pg__counter">{{ lastCounterCheckCrush }}</p>
        
        <div v-for="(obstacle, idx) in obstacles" :key="obstacle">
            <ObstacleApp 
                v-if="obstacle"
                :triggerToMoveObstacle="triggerToMoveObstacle"
                @clean="obstacles[idx] = false"
            />
        </div>
        <!-- {{ undead }}
        {{ counterCheckCrush }}
        {{ log }} -->
        <div class="pg__die" v-if="isDie">
            <p>СМЭРТЬ</p>
        </div>
    </div>
</template>
<script>
import ObstacleApp from './ObstacleApp.vue';
export default {
    components:{
        ObstacleApp,
    },
    props: {
        currentPerson: String,
    },
    data(){
        return{
            isJumping: false,
            isMove: false,
            isBeatObtacle: false,
            isDie: false,
            undead: false,
            
            triggerToMoveObstacle: 0,


            obstacles: [],
            timings: [600, 550, 700, 1100, 1200, 1800],
            stats: [],

            counterCheckCrush: 0,
            lastCounterCheckCrush: 0,
            log: '',
        }
    },
    mounted(){
        window.addEventListener('keydown', (e) => {
            console.log("!");
            if(this.isJumping){return}
            this.isJumping = true

            this.undead = true
            setTimeout(() => {
                this.undead = false
            }, 240)

            setTimeout(() => {
                this.isJumping = false
            }, 500)
        })
        this.loopMoveObstacle()

        setInterval(() => {
            this.stats.push([!!this.undead, this.counterCheckCrush > this.lastCounterCheckCrush])
            console.log(this.counterCheckCrush > this.lastCounterCheckCrush);
            this.lastCounterCheckCrush = this.counterCheckCrush
            if(this.stats.length > 140){ this.stats.shift() }
        }, 10)

    },
    methods: {
        moveObstacle(){
            this.obstacles.push(1)
            setTimeout(() => {
                this.checkCrush()
            }, 960)

        },
        loopMoveObstacle(){
            this.moveObstacle()
            if(Math.random() < 0.2){
                setTimeout(() => {
                    this.moveObstacle()
                }, 75)
            }
            setTimeout(()=> {
                this.loopMoveObstacle()
            }, this.timings[Math.floor(Math.random() * this.timings.length)])
        },
        checkCrush(){
            this.counterCheckCrush++
            this.log = `isJumping: ${this.isJumping}`
            if(!this.undead){
                this.$emit('die')
                this.isBeatObtacle = true
                setTimeout(()=> {
                    this.isDie = true
                }, 200)
            }
        },
    },
}
</script>
<style lang="scss">
    .pg{
        display: flex;
        height: 100%;
        align-items: flex-end;
        position: relative;
        overflow: hidden;
        &__back{
            width: 100%;
            height: 100%;
            position: absolute;
            z-index: 0;
            top: 0;
            left: 0;
            & > img{
                position: absolute;
                top: 0;
                left: 0;
                height: 100%;
                width: 4000px;
                min-width: 4000px;
                object-position: bottom;
                object-fit: cover;
                animation: backSlide 7s infinite linear;
            }
        }
        &__counter{
            position: absolute;
        }
        &__person{
            z-index: 0;
            &--jump{
                animation: jump .5s linear;
            }
            & > img{
                width: 100px;
                height: 100px;
                object-fit: contain;
            }
        }
        
        &__die{
            width: 100%;
            height: 100%;
            position: absolute;
            background: rgb(96, 9, 9);
            display: flex;
            justify-content: center;
            align-items: center;
            & > p{
                color: rgba(255, 255, 255, 0.716);
                font-weight: 700;
                font-size: 24px;
            }
        }

        &__indicator{
            display: flex;
            position: absolute;
            z-index: 10;
            border: 1px solid black;
            justify-content: flex-end;
            &_column{
                display: flex;
                flex-direction: column;
                &_sq{
                    width: 3px;
                    height: 4px;
                    background: yellow;
                    &--green{
                        background: green;
                    }
                    &--red{
                        background: red;
                    }
                }
            }
        }
    }
    @keyframes jump {
        0%, 100%{
            transform: translateY(0px);
        }
        28%, 72%{
            transform: translateY(-86px);
        }
        30%, 70%{
            transform: translateY(-90px);
        }
        46%, 54%{
            transform: translateY(-100px);
        }
    }
    @keyframes backSlide {
        0%{
            transform: translateX(0%);
        } 
        100% {
            transform: translateX(-2000px);
        }
    }

    
</style>