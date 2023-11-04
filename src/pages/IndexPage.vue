<template>
  <q-page class="q-pa-md column">
    <q-input outlined dense square v-model="date">
      <template v-slot:append>
        <q-icon name="event" class="cursor-pointer">
          <q-popup-proxy cover transition-show="scale" transition-hide="scale">
            <q-date v-model="date" mask="YYYY-MM-DD">
            </q-date>
          </q-popup-proxy>
        </q-icon>
      </template>
    </q-input>
    <div class="row q-mt-xs justify-between">
      <div>Ngày âm:</div>
      <div>{{lunarDate}}</div>
    </div>
    
    <div class="row q-mt-xs justify-between">
      <div>Năm tiếp thep:</div>
      <div>{{namAm}}</div>
    </div>

    <div class="row q-mt-xs justify-between">
      <div>Số ngày:</div>
      <div>{{dateNumber}}</div>
    </div>

    <q-input v-model="input" placeholder="Nhập số ngày" square outlined dense class="q-my-sm" type="number" @update:model-value="convertDateFromNumber"/>
    <div>{{ketQua}}</div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import Moment from 'moment'
import lunar from 'lunar-javascript'

export default defineComponent({
  name: 'IndexPage',
  data () {
    return {
      input: 100,
      ketQua: null,
      date: "2023-08-21",
      soNam: 1
    }
  },
  computed: {
    dateNumber () {

      return Moment().diff(this.date, 'days');
    },
    lunarDate () {
      let date = new Date(this.date)
      let ngayAm = lunar.Solar.fromYmd(date.getFullYear(), date.getMonth() + 1, date.getDate()).getLunar()
      return `${ngayAm.getDay()}/${ngayAm.getMonth()}/${ngayAm.getYear()}`
    },
    namAm () {
      let fotmatDate = new Date(this.date)
      let date = lunar.Lunar.fromYmd(fotmatDate.getFullYear() + this.soNam, fotmatDate.getMonth() + 1, fotmatDate.getDate()).getSolar()
      return `${date.getDay()}/${date.getMonth()}/${date.getYear()}`
    }
  },
  methods: {
    convertDateFromNumber () {
      let date = Moment(Moment(this.date).add(this.input - 1, 'days'))
      let fotmatDate = new Date(date)
      let ngayAm = lunar.Solar.fromYmd(fotmatDate.getFullYear(), fotmatDate.getMonth() + 1, fotmatDate.getDate()).getLunar()
      let valueLichAm = ` - Lịch âm: ${ngayAm.getDay()}/${ngayAm.getMonth()}/${ngayAm.getYear()}`
      this.ketQua = "Lịch dương: " + date.format("DD/MM/YYYY") + valueLichAm
    }
  },
  created () {
    this.convertDateFromNumber()
  }
})
</script>
