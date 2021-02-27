<template>
  <div class="hello">
    <header>
    <div class="contain">
      <div class="row header_photo">
        <div class="col-12 content">
          <div class="logo">
            <img src="../assets/img/logo.png" alt="">
          </div>
        </div>
        <div class="col-12 contact">
         <div class="d-block d-md-flex justify-content-center ml-3 m-md-auto">
            <div class="media mb-3">
            <img src="../assets/img/instagram-brands.svg" alt="">
            <img src="../assets/img/facebook-square-brands.svg" alt="">
          </div>
          <div class="list">
            <li><img src="../assets/img/phone-call.svg" alt="">02-22561385</li>
            <li><img src="../assets/img/email.svg" alt="">lofihouse@lofihouse.com.tw</li>
            <li><img src="../assets/img/home.svg" alt="">台北市大安區羅斯福路二段123號</li>
          </div>
         </div>
        </div>
      </div>
    </div>
  </header>
  <main>
    <room-card :roomLists="roomLists"></room-card>
  </main>
  </div>
</template>

<script>
import RoomCard from '../components/RoomCard.vue'
export default {
  name: 'Home',
  components: {
    RoomCard
  },
  data () {
    return {
      roomLists: []
    }
  },
  created () {
    let roomURL = 'https://challenge.thef2e.com/api/thef2e2019/stage6/rooms'
    let headers = {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
      'Authorization': 'Bearer 0r5IHmL0Fgaru9buA9vzxqSmL5BpDE7898j0zHFRFVamgPxch7NE5LoVxmG2'
    }
    this.axios.get(roomURL, { headers })
      .then((res) => {
        this.roomLists = res.data.items
        console.log(this.roomLists)
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
.hello{
  width: 100%;
}
.header_photo {
  background-image: url("../assets/img/room5_1.jpg");
  width: 100%;
  height: 100vh;
  background-size: cover;
  background-position: center;
}
.content{
  height: 200px;
  margin-top: 250px;
}
.logo{
  width: 150px;
  height: 150px;
  margin: auto;
}
.logo img{
  width: 100%;
}
.contact{
  margin-top: -200px;
}
.media{
  width: 95px;
}
.media img{
  width: 25%;
  margin-right: 5px;
}
li{
  list-style: none;
  width: 300px;
  display: flex;
  justify-content: flex-start;
  border-left: 3px solid white;
  padding-right: 5px;
  color: #fff;
  font-size: 14px;
}
li img{
  width: 25px;
  height: 25px;
  margin: 0px 10px 5px 20px;
}
</style>
