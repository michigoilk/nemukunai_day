<template>
  <div>
    <DatePicker @add="addDay" @remove="removeDay" :selected_days="selected_days"></DatePicker>
    <div id="days">
      <div v-for="(selected_day, key) in selected_days" :key="key">
        {{ selected_day }}<br>
        <br>
        <hr>
      </div>
    </div>
  </div>
</template>

<script>
import DatePicker from './components/DatePicker.vue'

export default {
  name: 'App',
  components: {
    DatePicker,
  },
  data(){
    return {
      selected_days: [],
    }
  },
  methods: {
    addDay(date){
      this.selected_days.push(date);
    },
    removeDay(date){
      this.selected_days = this.selected_days.filter((selected_day) => selected_day != date);
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#days{
  font-family: Serif;
  padding-top: 20px;
  margin-top: 20px;
  border-top: 1px solid #CCC;
  text-align: left;

}
</style>
