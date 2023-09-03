<template lang="">
    <div>
        <div class="obstacle" :class="{'obstacle--move': isMove}">
            <img src="@/assets/bochka.png" class="obstacle_img" :class="{'obstacle_img--beat': isBeatObtain}" alt="">
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            isMove: false,
            interval: null,
        }
    },
    props:{
        triggerToMoveObstacle: Number,
    },
    watch: {
        triggerToMoveObstacle(){
            // this.isMove = true
        }
    },
    methods: {
    },
    mounted(){
        this.isMove = true
        setTimeout(() => {
            this.$emit('clean')
        }, 5000)

    },
}
</script>
<style lang="scss">
    .obstacle{
        width: 70px;
        height: 70px;
        object-fit: contain;
        position: absolute;
        left: 0;
        bottom: 0;
        opacity: 0;
        &--move{
            animation: obstacleMove 1.3s linear forwards;
            opacity: 1;
        }
        &_img{
            transition: .1s;
            transition-delay: .06s;
            &--beat{
                transform: rotate(45deg) translateX(20px);
            }
        }
    }
    @keyframes obstacleMove {
        0%{
            transform: translateX(600px);
        }
        100%{
            transform: translateX(-100px);
        }
    }
</style>