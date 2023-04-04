<template>
  <!-- <Tutorial /> -->

  <div ref="calendar" class="calendar">
    <div>
      <button @click="prevMonth">이전</button>
      <span>{{ currentYear }}.{{ currentMonth }}</span>
      <button @click="nextMonth">이후</button>
    </div>
    <div class="week">
      <span class="date">일</span>
      <span class="date">월</span>
      <span class="date">화</span>
      <span class="date">수</span>
      <span class="date">목</span>
      <span class="date">금</span>
      <span class="date">토</span>
    </div>
    <div
      v-for="(week, weekIndex) in weeks"
      :key="'week' + weekIndex"
      class="week"
    >
      <span
        v-for="date in week"
        :key="weekIndex + 'date' + date"
        class="date"
        >{{ date }}</span
      >
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
  data() {
    return {
      weeks: [],
      dates: [],
      currentYear: 2023,
      currentMonth: 1,
      currentDate: 1
    }
  },
  mounted() {
    const date = new Date()
    this.currentYear = date.getFullYear()
    this.currentMonth = date.getMonth() + 1
    this.currentDate = date.getDate()
    this.setCalendar()
  },
  methods: {
    setCalendar() {
      this.dates = []
      this.weeks = []
      const date = new Date(this.currentYear, this.currentMonth, 0) // 넘겨받은 년월로 달력 세팅      }

      const utc = date.getTime() + date.getTimezoneOffset() * 60 * 1000 // utc 표준시 도출
      const kstGap = 9 * 60 * 60 * 1000 // 한국 kst 기준시간 더하기
      const today = new Date(utc + kstGap) // 한국 시간으로 date 객체 만들기(오늘)
      console.log(today)

      const currentYear = today.getFullYear()
      const currentMonth = today.getMonth() + 1
      const currentDate = today.getDate()
      const currentDay = today.getDay()
      console.log(`${currentYear} ${currentMonth} ${currentDate} ${currentDay}`)

      // 이전 달의 마지막 날 날짜와 요일 구하기
      const startDay = new Date(currentYear, currentMonth - 1, 0)
      console.log(startDay)

      const prevDate = startDay.getDate()
      const prevDay = startDay.getDay()
      console.log(prevDate)

      console.log(prevDay)

      // 이번 달의 마지막날 날짜와 요일 구하기
      const endDay = new Date(currentYear, currentMonth, 0)
      const nextDate = endDay.getDate()
      console.log('nextDate', nextDate)

      const nextDay = endDay.getDay()

      const calendar = this.$refs.calendar
      for (let i = prevDate - prevDay; i <= prevDate; i++) {
        this.dates.push(i)
      }
      for (let i = 1; i <= nextDate; i++) {
        this.dates.push(i)
      }
      for (let i = 0; i <= (6 - nextDay === 6 ? 6 : 6 - nextDay); i++) {
        this.dates.push(i + 1)
      }
      for (let index = 0; index < Math.floor(this.dates.length / 7); index++) {
        const week = []
        for (let i = 0; i < 7; i++) {
          week.push(this.dates[i + index * 7])
        }
        this.weeks.push(week)
      }

      console.log(this.weeks)
    },
    prevMonth() {
      this.currentMonth = this.currentMonth - 1
      this.setCalendar()
    },
    nextMonth() {
      this.currentMonth = this.currentMonth + 1
      this.setCalendar()
    }
  }
})
</script>

<style scoped>
.calendar {
  height: 700px;
  width: 700px;
  background-color: #32423453;
}
.date {
  margin: 10px;
  text-align: center;
  display: inline-block;
  width: 80px;
  height: 40px;
  background-color: #fff;
  border-radius: 3px;
}
.week span:first-child {
  color: red;
}
.week span:nth-child(7) {
  color: blue;
}
</style>
