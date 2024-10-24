<template>
  <q-page class="flex flex-center">
    <q-card class="my-card" v-for="(code, index) in dailycodes" :key="index">
      <img :src="tempsImage">

      <q-card-section>
        <div class="text-h6">Météo du jour</div>
        <div class="text-subtitle2">{{ wmoCodesReference[code].day.description }}</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        d
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import axios from 'axios'
import wmoCodes from 'src/data/wmo-codes-fr.json'
import { ref, onMounted } from 'vue'

defineOptions({
  name: 'IndexPage'
})

const temps = ref('')
const tempsImage =ref('')
const dailycodes =ref([])
const wmoCodesReference = ref(wmoCodes)

onMounted(async () => {
  const meteo = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min`)
  const code = meteo.data.current.weather_code
  temps.value = wmoCodes[code].day.description
  tempsImage.value = wmoCodes[code].day.image
  console.log(meteo)
  dailycodes.value = meteo.data.daily.weather_code

})
</script>
