<template>
<modal name="checkmodal" :resizable="true" :adaptive="true" height="auto" :maxWidth="430" :scrollable="true" :clickToClose="false">
    <div class="modal-wrapper">
      <div class="modal-container">
        <div class="modal-title">{{ modalTitle }}</div>
        <div class="dividing-line">\\\</div>
        <form class="booking-form" v-if="modalTitle === '預約時段'">
          <div class="form__group">
            <label for="name" class="form__label">姓名</label>
            <input v-model.trim="form.name" id="name" type="text" class="form__input" placeholder="請輸入姓名" />
          </div>
          <template v-if="$v.form.name.$dirty"><span v-if="!$v.form.name.required" class="warning">姓名為必填欄位</span></template>
          <div class="form__group">
            <label for="tel" class="form__label">電話</label>
            <input v-model.trim="form.tel" id="tel" type="tel" class="form__input" placeholder="請輸入電話" />
          </div>
          <template v-if="$v.form.tel.$dirty">
            <span class="warning" v-if="!$v.form.tel.required">電話為必填欄位</span>
            <span class="warning" v-if="!$v.form.tel.numeric">電話號碼必為數字</span>
          </template>
          <div class="form__group">
            <label for="tel" class="form__label">預約起訖</label>
            <div class="booking-picker">{{ range }}</div>
          </div>
        </form>
      </div>
      <div class="booking-days" v-if="modalTitle === '預約時段'">
        <div class="modal-container">
          <div class="booking-days__time">
            <p>平日時段</p>
            <p>{{ judgeWeekend.normal }} 夜</p>
          </div>
          <div class="booking-days__time">
            <p>假日時段</p>
            <p>{{ judgeWeekend.holiday }} 夜</p>
          </div>
        </div>
      </div>
      <div class="modal-container" v-if="modalTitle === '預約時段'">
        <div class="booking-price">= NT.{{ totalPrice }}</div>
      </div>
      <div class="booking-status modal-container" v-if="modalTitle !== '預約時段'">
        <img v-if="modalTitle === '預約成功'" >
        <p v-if="modalTitle === '預約失敗'">預約時間已被人預訂</p>
      </div>
      <div class="modal-btngroup modal-container">
        <button class="modal-btn btn-light" @click.prevent="hide()" v-if="modalTitle === '預約時段'">取消</button>
        <button class="modal-btn btn-dark" @click.prevent="submitForm()" v-if="modalTitle === '預約時段'">確定預約 <i v-if="sending" class="fas fa-spinner fa-spin"></i></button>
        <router-link to="/" style="margin-left: auto;" v-if="modalTitle === '預約成功'"><button class="modal-btn btn-dark">回首頁</button></router-link>
        <button style="margin-left: auto;" class="modal-btn btn-dark" @click.prevent="modalTitle = '預約時段'" v-if="modalTitle === '預約失敗'">返回</button>
      </div>
    </div>
  </modal>
</template>

<script>
import { required, numeric } from 'vuelidate/lib/validators'

export default {
  name: 'CheckoutModal',
  props: ['roomId', 'bookedDays', 'judgeWeekend', 'dayPrice', 'bookingDates', 'range'],
  data () {
    return {
      modalTitle: '預約時段',
      sending: false,
      form: { name: null, tel: null }
    }
  },
  computed: {
    totalPrice () {
      const vm = this
      let totalPrice = 0
      if (vm.judgeWeekend) {
        // 若超過一天最後一天不算錢
        if (vm.bookingDates && vm.bookingDates.length > 1) {
          const lastDay = new Date(vm.bookingDates[vm.bookingDates.length - 1]).getDay()
          if (lastDay === 0 || lastDay === 6) {
            vm.judgeWeekend.holiday -= 1
          } else {
            vm.judgeWeekend.normal -= 1
          }
        }
        totalPrice = vm.dayPrice.normal * vm.judgeWeekend.normal + vm.dayPrice.holiday * vm.judgeWeekend.holiday
      }
      return totalPrice
    }
  },
  validations: {
    form: {
      name: { required },
      tel: { required, numeric }
    }
  },
  methods: {
    formReset () {
      const vm = this
      vm.form.name = null
      vm.form.tel = null
    },
    submitForm () {
      const vm = this
      vm.$v.$touch()
      if (!vm.$v.$invalid) {
        vm.sending = true
        let roomURL = `https://challenge.thef2e.com/api/thef2e2019/stage6/room/${this.roomId}`
        const bookingOrder = { name: vm.form.name, tel: vm.form.tel, date: [...vm.bookingDates.map((day) => day.replace(/\//g, '-'))] }
        vm.axios
          .post(roomURL, bookingOrder, {
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json',
              'Authorization': 'Bearer 0r5IHmL0Fgaru9buA9vzxqSmL5BpDE7898j0zHFRFVamgPxch7NE5LoVxmG2'
            }
          })
          .then((res) => {
            if (res.status === 200) {
              vm.modalTitle = '預約成功'
            }
            vm.sending = false
          })
          .catch(() => {
            vm.modalTitle = '預約失敗'
            vm.sending = false
          })
      }
    },
    hide () {
      const vm = this
      vm.formReset()
      vm.$modal.hide('checkmodal')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
modal-wrapper {
  padding: 30px 0;
}
.modal-container {
  padding: 0 40px;
}
.modal-title {
  font-size: 24px;
  font-family: 'Noto Sans TC', sans-serif;
  letter-spacing: 2.5px;
  margin-bottom: 12px;
}
.modal-btngroup {
  display: flex;
  justify-content: space-between;
}
.modal-btn {
  padding: 8px 23px;
  font-size: 14px;
  letter-spacing: 1.5px;
  margin-top: 30px;
}
.btn-dark {
  background-color: #484848;
  color: #ffffff;
}
.btn-light {
  background-color: #d8d8d8;
  color: #6d7278;
}
.booking-form {
  margin-top: 12px;
}
</style>
