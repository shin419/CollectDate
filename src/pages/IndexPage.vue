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
    <div class="q-mt-xs">Ngày âm: {{lunarDate}}</div>
    <div>Số ngày: {{dateNumber}}</div>
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
      input: null,
      ketQua: null,
      date: "2023-08-21"
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
  }
})
</script>
