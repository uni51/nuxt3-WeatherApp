<script setup lang="ts">
const config = useRuntimeConfig();

const { data: city, error } = useAsyncData("city", async () => {
  let response;
  try {
    response = await $fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=Tokyo`,
      {
        params: {
          units: "metric",
          appid: config.WEATHER_APP_SECRET,
        },
      }
    );
  } catch (e) {}

  return response;
});
</script>

<template>
  <div class="h-screen relative overflow-hidden">
    {{ city }}
    <img />
    <div class="absolute w-full h-full top-0 overlay" />
    <div class="absolute w-full h-full top-0 p-48">
      <div class="flex justify-between">
        <div>
          <h1 class="text-7xl text-white">{{ city.name }}</h1>
          <p class="font-extralight text-2xl mt-2 text-white">Sunday Dec 9th</p>
          <img
            :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`"
            class="w-56 icon"
          />
        </div>
        <div>
          <p class="text-9xl text-white font-extralight">
            {{ city.main.temp }}°
          </p>
        </div>
      </div>
      <div class="mt-20">
        <input type="text" class="w-1/2 h-10" placeholder="Search a city..." />
        <button class="bg-sky-400 w-20 text-white h-10">Search</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.overlay {
  background-color: rgba(0, 0, 0, 0.5);
}

.icon {
  margin-left: -2.75rem;
  margin-top: -2.75rem;
}
</style>
