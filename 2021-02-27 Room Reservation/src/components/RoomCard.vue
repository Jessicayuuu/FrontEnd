<template>
  <section>
    <div class="container m-auto">
      <div class="row room">
        <router-link class="col-12 col-sm-6 col-md-6 col-lg-4 mb-5" v-for="room in roomLists" :key="room.id" :to="{ name:'RoomInfo', params:{ roomId: room.id}}">
          <div class="roomcard m-auto">
            <img :src="room.imageUrl" alt=""/>
            <div class="roomtext">
              <p>{{room.name}}</p>
              <p class="pricenormal">NT.{{room.normalDayPrice}}平日</p>
              <p class="priceholiday">NT.{{room.holidayPrice}}假日</p>
            </div>
          </div>
        </router-link>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'RoomCard',
  props: ['roomLists']
}
</script>

<style>
.room{
  margin-top: -50px;
}
.roomcard{
  width: 100%;
}
.roomcard img{
  width: 100%;
  height: 350px;
  position: relative;
  object-fit: cover;
  opacity: 0.8;
  transition: all 0.3s;
}
:hover.roomcard img{
  opacity: 1;
  padding-bottom: 30px;
}
:hover.roomcard .roomtext{
  padding-bottom: 70px;
  background-color: #E1DCD9;
  color: black;
  transition: all 0.3s;
}
:hover.roomcard .roomtext p{
  position: absolute;
  left: 15px;
  bottom: 20px;
  transition: all 0.3s;
  color: black;
}
:hover.roomcard .roomtext .pricenormal{
  display: unset;
  position: absolute;
  left: 15px;
  bottom: -5px;
  font-size: 17px;
  letter-spacing: 1.5px;
  transition: all 0.5s;
}
:hover.roomcard .roomtext .priceholiday{
  display: unset;
  position: absolute;
  right: -20px;
  bottom: -3px;
  font-size: 14px;
  margin-left: 50px;
  transition: all 0.5s;
}
.roomtext{
  width: 92%;
  height: 60px;
  background-color: #8F8681;
  color: black;
  position: absolute;
  bottom: 0px;
}
.roomtext p{
  position: absolute;
  left: 15px;
  bottom: 0px;
}
.pricenormal,.priceholiday{
  display: none;
}
@media (max-width: 1200px) {
  .roomtext{
    width: 91%;
  }
}
@media (max-width: 768px) {
  .roomtext{
    width: 90%;
  }
}
@media (max-width: 576px) {
  .roomtext{
    width: 94%;
  }
}
@media (max-width: 375px) {
  .roomtext{
    width: 92%;
  }
}
@media (max-width: 320px) {
  .roomtext{
    width: 91%;
  }
}
</style>
