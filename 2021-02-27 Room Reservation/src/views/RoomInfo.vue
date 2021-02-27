<template>
  <div v-if="roomDatas.name">
    <header class="d-flex">
      <div class="img_main">
        <img :src="roomDatas.imageUrl[0]">
      </div>
      <div class="img_side">
        <img :src="roomDatas.imageUrl[1]">
        <img :src="roomDatas.imageUrl[2]">
      </div>
    </header>
    <main>
      <div class="container m-auto">
        <div class="col-12 d-block d-lg-flex">
        <div class="col-12 col-sm-12 col-md-12 col-lg-6 col-xl-6">
          <div class="room_info">
            <h2 class="room_name">{{ roomDatas.name }}</h2>
             <div class="col-12 price d-lg-none">
              <p class="price_normal">NT.{{ roomDatas.normalDayPrice }} / 平日</p>
              <p class="price_holiday">NT.{{ roomDatas.holidayPrice }} / 假日</p>
            </div>
            <date-picker class="d-lg-none" @bookingDates="emitBookingDates" :bookedDays="bookedDays"></date-picker>
            <ul class="room_description">
              <li>客房人數限制: {{ roomDatas.descriptionShort.GuestMin }} - {{ roomDatas.descriptionShort.GuestMax }}人</li>
              <li>床型: {{ roomDatas.descriptionShort.Bed[0] }}</li>
              <li>衛浴數量： {{ roomDatas.descriptionShort['Private-Bath'] }}間</li>
              <li>房間大小: {{ roomDatas.descriptionShort.Footage }}平方公尺</li>
              <li>{{ roomDatas.description }}</li>
            </ul>
          </div>
          <hr>
          <div class="room_check">
            <div class="check">
              <p class="check_text">Check In</p>
              <p class="check_time">{{ roomDatas.checkInAndOut.checkInEarly }} - {{ roomDatas.checkInAndOut.checkInLate }}</p>
            </div>
            <div class="check">
              <p class="check_text">Check Out</p>
              <p class="check_time">{{ roomDatas.checkInAndOut.checkOut }}</p>
            </div>
          </div>
          <ul class="d-flex flex-wrap mx-auto my-3 features">
            <li :style="isFeature(features['Wi-Fi'])"><img class="features_icon" src="../assets/img/features/Wi-Fi.svg">Wi-Fi</li>
            <li :style="isFeature(features['Television'])"><img class="features_icon" src="../assets/img/features/Television.svg">電視</li>
            <li :style="isFeature(features['Great-View'])"><img class="features_icon" src="../assets/img/features/Great-View.svg">漂亮的景色</li>
            <li :style="isFeature(features['Breakfast'])"><img class="features_icon" src="../assets/img/features/Breakfast.svg">早餐</li>
            <li :style="isFeature(features['Air-Conditioner'])"><img class="features_icon" src="../assets/img/features/Air-Conditioner.svg">空調</li>
            <li :style="isFeature(features['Smoke-Free'])"><img class="features_icon" src="../assets/img/features/Smoke-Free.svg">禁止吸菸</li>
            <li :style="isFeature(features['Mini-Bar'])"><img class="features_icon" src="../assets/img/features/Mini-Bar.svg">Mini Bar</li>
            <li :style="isFeature(features['Refrigerator'])"><img class="features_icon" src="../assets/img/features/Refrigerator.svg">冰箱</li>
            <li :style="isFeature(features['Child-Friendly'])"><img class="features_icon" src="../assets/img/features/Child-Friendly.svg">適合兒童</li>
            <li :style="isFeature(features['Room-Service'])"><img class="features_icon" src="../assets/img/features/Room-Service.svg">Room Service</li>
            <li :style="isFeature(features['Sofa'])"><img class="features_icon" src="../assets/img/features/Sofa.svg">沙發</li>
            <li :style="isFeature(features['Pet-Friendly'])"><img class="features_icon" src="../assets/img/features/Pet-Friendly.svg">可攜帶寵物</li>
          </ul>
        </div>
        <div class="col-12 col-sm-12 col-md-12 col-lg-6 col-xl-6 room_booking">
            <div class="col-12 price d-none d-lg-block">
              <p class="price_normal">NT.{{ roomDatas.normalDayPrice }} / 平日</p>
              <p class="price_holiday">NT.{{ roomDatas.holidayPrice }} / 假日</p>
            </div>
            <date-picker class="d-none d-lg-block" @bookingDates="emitBookingDates" :bookedDays="bookedDays"></date-picker>
            <check-modal :range="range" :dayPrice="dayPrice" :bookingDates="bookingDates" :judgeWeekend="judgeWeekend" :roomId="roomId" :bookedDays="bookedDays"></check-modal>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import RoomCard from '@/components/RoomCard.vue'
import DatePicker from '@/components/DatePicker.vue'
import CheckModal from '@/components/CheckModal.vue'
export default {
  name: 'RoomInfo',
  components: {
    RoomCard,
    DatePicker,
    CheckModal
  },
  data () {
    return {
      roomDatas: [],
      roomId: null,
      bookingDates: null,
      judgeWeekend: { normal: 0, holiday: 0 },
      range: ''
    }
  },
  computed: {
    features () {
      return this.roomDatas.amenities
    },
    dayPrice () {
      return { normal: this.roomDatas.normalDayPrice, holiday: this.roomDatas.holidayPrice }
    }
  },
  methods: {
    emitBookingDates (data) {
      this.bookingDates = data.bookingDates
      this.judgeWeekend = data.judgeWeekend
      this.range = data.range
    },
    isFeature (feature) {
      if (feature === true) {
        return 'opacity:1'
      }
      return 'opacity:0.3'
    }
  },
  created () {
    this.roomId = this.$route.params.roomId
    let roomURL = `https://challenge.thef2e.com/api/thef2e2019/stage6/room/${this.roomId}`
    let headers = {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
      'Authorization': 'Bearer 0r5IHmL0Fgaru9buA9vzxqSmL5BpDE7898j0zHFRFVamgPxch7NE5LoVxmG2'
    }
    this.axios.get(roomURL, { headers })
      .then((res) => {
        this.roomDatas = res.data.room[0]
        this.bookedDays = res.data.booking.map((day) => day.date.replace(/-/g, '/'))
        // console.log(this.roomDatas)
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
header{
  height: 600px;
}
.img_main{
  width: 70%;
  height: 100%;
}
.img_main img{
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.img_side{
  width: 30%;
  height: 100%;
}
.img_side img{
  width: 100%;
  height: 50%;
  object-fit: cover;
}
.container{
  display: flex;
  flex-wrap: wrap;
}
.room_name {
    font-family: 'Noto Sans TC', sans-serif;
    font-size: 2.5rem;
    font-weight: 500;
    margin-bottom: 30px;
}
ul{
  display: inline-block;
  text-align: start;
}
li{
  list-style: none;
}
.room_description{
  padding: 0px 30px;
}
.room_check{
  width: 80%;
  display: flex;
  justify-content: space-between;
  padding: 0px 30px;
}
.check{
  width: 150px;
}
.check_text{
  font-size: 16px;
  font-family: 'Noto Sans TC', sans-serif;
  font-weight: 200;
  text-align: start;

}
.check_time{
  font-size: 21px;
  font-family: 'Noto Sans TC', sans-serif;
  font-weight: 200;
  text-align: start;
}
.features{
  width: 80%;
  background:#f0f0f0;
  padding: 40px 0px 40px 20px;
  /* margin: 40px 0px; */
  /* display: flex; */
  /* flex-wrap: wrap; */
}
.features_icon{
  width: 25px;
  height: 25px;
  margin-bottom: 12px;
  margin-right: 12px;
}
.features>li{
  display: flex;
  flex: 0 0 33.3%;
  font-family: 'Noto Sans TC', sans-serif;
  font-size: 12px;
}
.price{
  font-family: 'Noto Sans TC', sans-serif;
  font-weight: 500;
  padding: 0 30px;
}
.price_normal{
  font-size: 30px;
}
</style>
