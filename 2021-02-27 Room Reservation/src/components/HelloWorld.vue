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
         <div class="contactAll d-block">
            <div class="media">
            <img src="../assets/img/instagram-brands.svg" alt="">
            <img src="../assets/img/facebook-square-brands.svg" alt="">
          </div>
          <div >
            <li><img src="../assets/img/phone-call.svg" alt="">02-22561385</li>
            <li><img src="../assets/img/email.svg" alt="">lofihouse@lofihouse.com.tw</li>
            <li><img src="../assets/img/home.svg" alt="">台北市大安區羅斯福路二段123號</li>
          </div>
         </div>
        </div>
      </div>
    </div>
  </header>
  <section>
    <div class="container m-auto">
      <div class="row room">
        <div class="col-4">
          <div class="roomtype m-auto">
            <img src="../assets/img/room1_1.jpg" alt="">
            <div class="roomcard">
              <p>Single Room</p>
            </div>
          </div>
        </div>
        <div class="col-4">
          <div class="roomtype m-auto">
            <img src="../assets/img/room2_1.jpg" alt="">
          </div>
        </div>
        <div class="col-4">
           <div class="roomtype m-auto">
            <img src="../assets/img/room3_1.jpg" alt="">
          </div>
        </div>
      </div>
    </div>
  </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
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
.contactAll{
  /* display: flex;
  justify-content: center;
  align-items: center; */
  width: 500px;
  margin: auto;
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
.room{
  margin-top: -50px;
}
.roomtype{
  width: 300px;
}
.roomtype img{
  width: 100%;
  height: 350px;
  position: relative;
}
.roomcard{
  width: 300px;
  height: 60px;
  background-color: wheat;
  position: absolute;
  bottom: 0px;
}
</style>
