<template>
<div class="parent">
    <div class="control">
        <p @click="prevMonth()" class="button">&lt;</p>
        <p class="current">{{ currentYear }}年 {{ currentMonth }}月</p>
        <p @click="nextMonth()" class="button">&gt;</p>
    </div>
    <div class="calender">
        <div class="week">
            <div v-for="(week, w_key) in week_days" :key="w_key">{{ week }}</div>
        </div>
        <div v-for="(week, w_key) in getCalender" :key="w_key" class="week">
            <div v-for="(day, d_key) in week" :key="d_key" :class="{'selected':day === null ? false : isAppended(makeDate(day, d_key).val)}" @click="clickDate(day, d_key)">{{ day }}</div>
        </div>
    </div>
</div>
</template>
<script>
export default {
    name: 'DatePicker',
    props:{
        selected_days: Array,
    },
    data() {
        return{
            currentYear: new Date().getFullYear(),
            currentMonth: new Date().getMonth() + 1,
            week_days: ['日', '月', '火', '水', '木', '金', '土'],
        }
    },
    methods: {
        nextMonth(){
            if(this.currentMonth >= 12){
                this.currentMonth = 1;
                this.currentYear++;
            }else{
                this.currentMonth++;
            }
        },
        prevMonth(){
            if(this.currentMonth <= 1){
                this.currentMonth = 12;
                this.currentYear--;
            }else{
                this.currentMonth--;
            }
        },
        clickDate(day, d_key){
            if(day === null || d_key === null){
                return null;
            }

            let date = this.makeDate(day, d_key);

            if(this.isAppended(date.val)){
                this.$emit('remove', date);
            }else{
                this.$emit('add', date);
            }
        },
        makeDate(day, d_key){
            if(day === null || d_key === null){
                return null;
            }
            let month = ('0' + this.currentMonth).slice( -2 );
            day = ('0' + day).slice( -2 );
            return {
                'val': this.currentYear + '/' + month + '/' + day,
                'show': this.currentYear + '年' + month + '月' + day + '日 (' + this.week_days[d_key] + ')',
            }
        },
        isAppended(date){
            return this.selectedDays.filter((selected_day) => selected_day.val == date).length > 0;
        }
    },
    computed: {
        getCalender(){
            let calender_array = [];
            let tmp_week = [];
            for(let i = 0;i < this.getFirstWeekDay; i++){
                tmp_week.push(null);
            }
            for(let day = 1; day <= this.getLastDay; day++){
                if(tmp_week.length >= 7){
                    calender_array.push(tmp_week);
                    tmp_week = [];
                }
                tmp_week.push(day);
            }
            calender_array.push(tmp_week);

            return calender_array;
        },
        getFirstWeekDay(){
            let first_day = new Date(this.currentYear, this.currentMonth - 1, 1);
            return first_day.getDay();
        },
        getLastDay(){
            let last_day = new Date(this.currentYear, this.currentMonth , 0);
            return last_day.getDate();
        },
        selectedDays(){
            let selected = this.selected_days;
            return selected.sort((a, b) =>  a.val > b.val);
        },
    },
}
</script>
<style scoped>
    .parent{
        display: inline-block;
    }
    .control{
        display: flex;
        margin: 10px 0;
        line-height: 30px;
    }
    .button{
        width: 20px;
    }
    .current{
        flex: 1;
    }
    .calender{
        display: flex;
        flex-direction: column;
        gap: 10px;
    }
    .week{
        display: flex;
        gap: 10px;
        border-bottom: 1px solid #CCC;
    }
    .calender .week div{
        width: 30px;
        height: 30px;
        line-height: 30px;
    }
    .selected{
        background-color: #CCC;
        border-radius: 15px;
    }
</style>