<script setup>
  const citySearch = ref('Otawa')
  const inputValue = ref('')
  const { data: city, error } = useFetch(() => `http://api.weatherapi.com/v1/current.json?key=96191023498a4f13b4f122145241902&q=${citySearch.value}&aqi=no`)
  const submitCity = () => {
    // citySearch.value = e.target
    citySearch.value = inputValue.value
  }
</script>

<template>
  <div class="h-screen relative overflow-hidden">
    <img />
    <div class="absolute w-full h-full top-0 overlay"/>
    <div class="absolute w-full h-full top-0 p-48">
        <div class="flex justify-between">
          <div>
          <h1 class="text-7xl text-white">{{ city?.location.name }}</h1>
          <p class="font-extralight text-2xl text-white mt-2">{{ new Date(city?.location.localtime).toDateString().slice(0,15) }}</p>
          <img class="w-48 icon" :src="`https://${city.current.condition.icon}`"/>
          <img />
        </div>
        <div>
          <p class="text-9xl text-white font-extralight">
            {{ city?.current.temp_c }}°
          </p>
        </div>
      </div>
      <div class="mt-20">
        <input type="text" class="w-1/2 h-10" placeholder="Search a city" v-model="inputValue"/>
        <button class="bg-sky-400 w-20 text-white h-10" @click="submitCity()">Search</button>
      </div>
    </div>
  </div>
</template>

<style>
  .overlay {
    background-color: rgb(0, 0, 0, 0.5);
  }
</style>
