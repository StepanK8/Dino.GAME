<template>
    <div class="main">
        <div class="main__wrap">
            <PlayGround 
                v-if="isShowPlayground"
                @die="die"
                :currentPerson="currentPerson"
            />

            <div v-if="isShowRestartButton" class="main__restart">
                <p>
                    Еще раз?
                </p>
                <button @click="restart">
                    RESTART
                </button>
            </div>
            
            <div v-if="isShowPick" class="main__pick">
                <p>Выбери себя</p>
                <div class="main__pick_grid">
                    <div v-for="person in persons" @click="choose(person)" class="main__pick_person">
                        <img :src="person" alt="">
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>
<script>
import PlayGround from '@/components/PlayGround.vue'
export default {
    components:{
        PlayGround,
    },
    data(){
        return{
            isShowPlayground: false,
            isShowRestartButton: false,
            isShowPick: true,
            persons: [
                './persons/sonic.gif',
                './persons/trump.gif',
                './persons/dolb.gif',
                './persons/dog.gif',
                './persons/batman.gif',
                './persons/pickachu.gif',
            ],
            currentPerson: null,
        }
    },
    methods: {
        die(){
            this.isShowPlayground = false 
            this.isShowRestartButton = true
        },
        restart(){
            this.isShowRestartButton = false
            this.isShowPlayground = true
        },  
        choose(person){
            this.currentPerson = person
            this.isShowPlayground = true
            this.isShowPick = false
        }
    }
}
</script>
<style lang="scss">
    .main{
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100vh;
        &__wrap{
            width: 600px;
            height: 320px;
        }
        &__restart{
            margin: auto;
            width: 400px;
            margin-top: 50px;
            margin-bottom: auto;
            & > p{
                font-size: 20px;
                text-align: center;
            }
            & > button{
                margin-top: 20px;
                padding: 4px 20px;
                border-radius: 10px;
                background: rgb(16, 182, 16);
                color: white;
                cursor: pointer;
                &:hover{
                    background: rgb(7, 158, 7);
                }
            }

        }
        &__pick{
            background: black;
            height: 100%;
            & > p{
                color: white;
                padding: 10px;
                text-align: center;
            }
            &_grid{
                justify-content: center;
                grid-template-columns: repeat(3,120px);
                gap: 2px;
                display: grid;

            }
            &_person{
                width: 120px;
                height: 120px;
                background: white;
                & > img{
                    object-fit: contain;
                    width: 100%;
                    height: 100%;
                }
            }
        }
    }
    
</style>