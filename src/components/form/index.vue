<script setup lang="ts">
import axios from 'axios'
import {ref, watch} from "vue";

const emit = defineEmits(['response'])
const inputValue = ref()
const deliveryResponse = ref([])
const errorNotFounded = ref(false)
const checkedCity = ref(false)
const popularCities = [
  'Nur-Sultan',
  'Almaty',
  'Shymkent',
  'Atyrau',
  'Aktau',
  'Zhana Turmis',
  'Karaganda',
  'Kentau',
  'Aitei',
  'Pavlodar',
]

const getCities = () => {
  axios.get('https://test-frontend.stage.mechta.market/delivery/check', {
    params: {
      search: inputValue.value.toString().toLowerCase(),
    }
  })
      .then((response) => {
        deliveryResponse.value = response?.data
        emit('response', response?.data)
        errorNotFounded.value = false
        checkedCity.value = true
      })
      .catch((error) => {
        if (error?.response?.data?.error === 'not found') {
          emit('response', [])
          deliveryResponse.value = []
          errorNotFounded.value = true
          checkedCity.value = false
        } else {
          console.log(error)
        }
      })
}

const setPopularCity = (city: string) => {
  errorNotFounded.value = false
  inputValue.value = city.toLowerCase()
  window.scrollTo(0, 0)
}

const clearInput = () => {
  emit('response', [])
  deliveryResponse.value = []
  inputValue.value = null
  errorNotFounded.value = false
  checkedCity.value = false
}

watch(inputValue, () => {
  if (inputValue.value === '' || inputValue.value === null) {
    emit('response', [])
    errorNotFounded.value = false
    checkedCity.value = false
  } else {
    inputValue.value = inputValue.value.toString().toLowerCase()
    getCities()
  }
})
</script>

<template>
  <div>
    <form @submit.prevent="getCities" class="search" :class="{ 'error': errorNotFounded && !checkedCity }">
      <input v-model="inputValue" type="text" placeholder="Enter name of the city" required>
      <img @click="clearInput" v-if="errorNotFounded" src="/img/clear.svg" alt="photo">
      <img @click="clearInput" v-else-if="checkedCity" src="/img/clearBlue.svg" alt="photo">
      <button v-else type="submit">Enter</button>
    </form>
    <p v-if="errorNotFounded" class="error-text">We didn’t found such city. Please check spelling</p>
    <div class="cities" :class="{ 'display-cities': checkedCity }">
      <p class="cities__title">Most popular cities</p>
      <div class="cities__list">
        <p @click="setPopularCity(i)" v-for="i in popularCities" :key="i" :class="{ active: i.toLowerCase() === inputValue }">{{i}}</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped src="./index.scss" />
