<template>
  <div class="calendar">
    <div class="calendar__header">
      <div class="calendar__header-top">
        <button class="calendar__control calendar__control_left" @click.prevent="prevMonth"></button>
        <div class="calendar__title">
          {{getTitle}}
        </div>
        <button class="calendar__control calendar__control_right" @click.prevent="nextMonth"></button>
      </div>
      <div class="calendar__header-bottom">
        <div class="calendar__list-name-days">
          <div class="calendar__list-name-day" v-for="name in listNameDays" :key="name">
            {{name}}
          </div>
        </div>
      </div>
    </div>
    <div class="calendar__days">
        <CalendarDay position="prev" v-for="day in prevDays" :day="day" :key="`prevDays_${day}`"/>
        <CalendarDay v-for="day in currentDays" :day="day" :month="currentShortMonthName" :key="`currentDays_${day}`"/>
        <CalendarDay position="next" v-for="day in countDaysNextMonth" :day="day" :month="nextShortMonthName" :key="`nextDays_${day}`"/>
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs'
import {DATE_FORMAT, LIST_NAME_DAYS} from './constants'
import CalendarDay from "@/components/Calendar/Components/CalendarDay";

export default {
  name: "Calendar",
  components: {CalendarDay},
  data: () => ({
    listNameDays: LIST_NAME_DAYS
  }),
  props: {
    date: {
      type: String,
      default: dayjs().format(DATE_FORMAT)
    }
  },
  computed: {
    getTitle(){
      return `${this.currentMonthName} ${this.currentYear}`
    },
    currentDays(){
      return dayjs(this.date).daysInMonth()
    },
    currentShortMonthName(){
      return dayjs(this.date).format('MMM')
    },
    currentMonthName(){
      return dayjs(this.date).format('MMMM')
    },
    currentYear(){
      return dayjs(this.date).format('YYYY')
    },
    nextShortMonthName(){
      return dayjs(this.date).add(1, 'month').format('MMM')
    },
    countDaysNextMonth(){
      return 14 - dayjs(this.date).set('date', dayjs(this.date).daysInMonth()).day()
    },
    prevDays(){
      const countDaysInPrevMonth = dayjs(this.date).add(-1, 'month').daysInMonth()
      const countShowsDaysInPrevMonth = dayjs(this.date).add(-1, 'month').date(countDaysInPrevMonth).day()
      const arrayDays = []
      for(let i = 0; i < countShowsDaysInPrevMonth; i++){
        arrayDays.push(countDaysInPrevMonth - i)
      }
      return arrayDays.reverse()
    }
  },
  methods: {
    prevMonth(){
      this.onUpdate( dayjs(this.date).add(-1, 'month').format(DATE_FORMAT))
    },
    nextMonth(){
      this.onUpdate( dayjs(this.date).add(1, 'month').format(DATE_FORMAT))
    },
    onUpdate(date){
      this.$emit('update', date)
    }
  }
};
</script>

<style lang="scss">
  .calendar{
    width: 250px;
    border: 1px solid rgba(0,0,0,0.3);
    &__days{
      display: grid;
      grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
    }
    &__header-top{
      display: flex;
      justify-content: space-between;
      padding: 10px 15px;
      align-items: center;
    }
    &__control{
      position: relative;
      width: 12px;
      height: 12px;
      cursor: pointer;
      background: unset;
      border: unset;
      &:after{
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        border-left: 3px solid rgba(0,0,0,0.3);
        border-top: 3px solid rgba(0,0,0,0.3);
        width: 100%;
        height: 100%;
        border-radius: 2px;
        box-sizing: border-box;
      }
      &_left:after{
        transform: rotate(315deg);
      }
      &_right:after{
        transform: rotate(135deg);
      }
    }
    &__list-name-days{
      display: grid;
      grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
      font-size: 10px;
    }
    &__list-name-day{
      text-align: center;
      padding: 5px 0;
    }
    &__header{
      border-bottom: 1px solid rgba(0,0,0,0.3);
    }
  }
  .day{
    display: flex;
    align-items: center;
    height: 35px;
    justify-content: center;
    position: relative;
    &.next, &.prev {
      background: beige;
    }
    &__title{
      position: absolute;
      left: 50%;
      top: 0;
      transform: translateX(-50%);
      font-size: 10px;
    }
  }
</style>
