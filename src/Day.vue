<template>
    <div class="day">
        <div class="date"><span>{{day.date.getDate()}}</span> <span class="month">({{day.date.getMonth()+1}}월)</span></div>
        <div class="list">
            <div class="item" v-for="todo in day.list">{{todo.name}}</div>
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
        }
    }
</script>

<style scoped>
    .day {
        width:100%;
        height:100%;
        display: flex;
        justify-content: flex-start;
        flex-direction: column;
        box-shadow: 2px 2px 2px 2px #ddd;
    }

    .date {
        margin: 10px 10px; 
    }

    .date > span:nth-child(1) {
        font-size: 24px;
    }




</style>