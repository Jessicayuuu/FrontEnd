<template>
  <div class="col-12">
    <VueHotelDatepicker ref="datePickInfo" :disabledDates="[...bookedDays]" :mobile="'mobile'" :weekList="week" :monthList="month" @update="passingDates()" :minDate="tomorrow"></VueHotelDatepicker>
    <button @click="show()" class="room-booking__btn">
      <div class="room-booking__btn-bg"></div>
      預約時段
    </button>
  </div>
</template>

<script>
/* eslint-disable */
import VueHotelDatepicker from '@northwalker/vue-hotel-datepicker'
import Moment from 'moment'
import { extendMoment } from 'moment-range'

const moment = extendMoment(Moment)
export default {
  name: 'date-picker',
  props: ['bookedDays'],
  components: {
    VueHotelDatepicker
  },
  data () {
    return {
      week: ['日', '一', '二', '三', '四', '五', '六'],
      month: ['1 /', '2 /', '3 /', '4 /', '5 /', '6 /', '7 /', '8 /', '9 /', '10 /', '11 /', '12 /'],
      pickerDay: [],
      isActive: true,
      hasError: false
    }
  },

  computed: {
    tomorrow () {
      return Moment().add(1, 'days').format('YYYY/MM/DD')
    },
    startEnd () {
      return this.$refs.datePickInfo.value
    },
    // 計算訂房日期
    bookingDates () {
      if (this.$refs.datePickInfo.value !== '') {
        let startAndEnd = this.$refs.datePickInfo.value.split(' ~ ').map((date) => date.replace(/\//g, '-'))
        let range = moment.range(startAndEnd[0], startAndEnd[1])
        let days = Array.from(range.by('day')).map((m) => m.format('YYYY-MM-DD'))
        let newBookedDays = this.bookedDays.map((day) => day.replace(/\//g, '-'))
        this.pickerDay = days.filter((day) => !newBookedDays.includes(day))
      }
      return this.pickerDay
    },
    // 判斷訂房平日/假日
    judgeWeekend () {
      let dates = { normal: 0, holiday: 0 }
      if (this.bookingDates.length > 0) {
        this.bookingDates.forEach((date) => {
          let week = new Date(date).getDay()
          if (week === 0 || week === 6) {
            dates.holiday += 1
          } else {
            dates.normal += 1
          }
        })
      }
      return dates
    }
  },
  methods: {
    passingDates () {
      // change 無法即時抓到數據，使用setTimeout延遲抓取
      setTimeout(() => {
        this.$emit('bookingDates', { bookingDates: this.bookingDates, judgeWeekend: this.judgeWeekend, range: this.startEnd })
      }, 0)
    },
    styleChange () {
      document.querySelector('.vhd-calendar-header').remove()
      document.querySelector('.vhd-calendar-footer').remove()
      document.querySelector('.vhd-input').remove()
    },
    show () {
      if (this.startEnd !== '') {
        this.$modal.show('checkmodal')
      // console.log(this.$modal)
      } else {
        this.$toasted.error('請選擇日期範圍', { duration: 2000 })
      }
    }
  },
  mounted () {
    this.$refs.datePickInfo.active = true
    setTimeout(() => {
      this.styleChange()
    }, 0)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.vhd-picker {
    z-index: 100;
    position: relative !important;
    width: 648px;
    min-height: 420px;
    padding: 24px;
    background-color: #fff;
    border-radius: 6px;
    -webkit-box-shadow: 0 2px 30px 0 rgba(0,0,0,.27);
    box-shadow: 0 2px 30px 0 rgba(0,0,0,.27);
    overflow: hidden;
}
</style>
