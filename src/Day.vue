<template>
    <div class="day" @click="openPopup">
        <div class="date">
            <span class="number">{{day.date.getDate()}}</span>
            <span class="month">({{day.date.getMonth() + 1}}월)</span>
        </div>
        <div class="list">
            <div class="item" v-for="(todo, idx) in day.list" @click.stop="openEditPopup(idx)">{{todo.name}}</div>
        </div>
    </div>
</template>

<script>
    export default {
        name:'day',
        props:['day'],
        data(){
            return {
                
            }
        },
        methods:{
            openPopup(){
                this.$emit("open");
            },
            openEditPopup(idx){
                this.$emit("edit", event, idx, this.day.idx);
            }
        }
    }
</script>

<style scoped>

::-webkit-scrollbar {
    width: 5px;
    height: 5px;
    background-color: rgba(0, 0, 0, 0.5);
}

::-webkit-scrollbar-thumb {
    background-color: rgba(255,  255, 255, 0.5);
}
    .day {
        width:100%;
        height:100%;
        display: flex;
        justify-content: flex-start;
        flex-direction: column;
        border: 2px solid #67493C;
        border-right: 0;
        border-bottom: 0;
    }

    .date {
        padding:8px;
    }

    .list {
        width: 200px;
        height: 120px;
        overflow-y: auto;
    }

    .number {
        font-size:25px;
        font-weight: bold;
    }

    .month {
        font-size:15px;
        color:#555;
    }

    .item {
        background-color: #67493C;
        color:#fff;
        border-radius: 0.25rem;
        padding:0.2rem 0.25rem;
        margin:4px;
        font-size: 18px;
    }

</style>