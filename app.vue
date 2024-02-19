<script setup>
  const citySearch = ref('Belgrade')
  const inputValue = ref('')
  const background = ref('')
  // const { data: city, error } = useFetch(() => `http://api.weatherapi.com/v1/current.json?key=96191023498a4f13b4f122145241902&q=${citySearch.value}&aqi=no`)


  const { data: city, error } = useAsyncData('city', async () => {
    const response = await $fetch(`http://api.weatherapi.com/v1/current.json?key=96191023498a4f13b4f122145241902&q=${citySearch.value}&aqi=no`)

    const temp = response.current.temp_c

    if (temp <= -10) {
        background.value =
          "https://images.unsplash.com/photo-1483664852095-d6cc6870702d?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80";
      } else if (temp > -10 && temp <= 0) {
        background.value =
          "https://images.unsplash.com/photo-1476820865390-c52aeebb9891?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80";
      } else if (temp > 0 && temp <= 10) {
        background.value =
          "https://images.unsplash.com/photo-1560258018-c7db7645254e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=4032&q=80";
      } else {
        background.value =
          "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3546&q=80";
      }
    
    return response
  }, {
    watch: [citySearch]
  })
  const submitCity = () => {
    citySearch.value = inputValue.value
  }
</script>

<template>
  <div class="h-screen relative overflow-hidden">
    <img :src="background"/>
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
