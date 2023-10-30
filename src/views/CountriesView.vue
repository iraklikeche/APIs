<template>
  <main class="flex items-center justify-center bg-[#3d64e6] px-24 py-24">
    <div class="flex w-1/2 flex-col rounded-xl bg-white px-10 py-12">
      <div class="flex items-center justify-center gap-8">
        <input
          placeholder="Type a Country"
          v-model="input"
          class="w-full rounded-lg border-b-2 border-[#3d64e6] bg-transparent p-2 px-4 placeholder-black"
        />
        <button
          @click="getCountry"
          class="mt-4 rounded-full bg-[#3d64e6] px-8 py-2 text-center tracking-wider text-white transition-opacity duration-500 hover:opacity-50"
        >
          Search
        </button>
      </div>

      <div v-for="info in data" class="py-8">
        <div class="flex justify-center">
          <img :src="info.flags.png" />
        </div>
        <h1
          class="mt-8 text-center text-xl font-bold uppercase tracking-widest"
        >
          {{ info.name.official }}
        </h1>

        <div class="mt-8">
          <ul class="">
            <li class="font-bold">
              Capital: <span class="opacity-70">{{ info.capital[0] }}</span>
            </li>
            <li class="font-bold">
              Population: <span class="opacity-70">{{ info.population }}</span>
            </li>
            <li class="font-bold">
              Currency:
              <span class="opacity-70">{{ info.currencies }}</span>
            </li>
            <li class="font-bold">
              Common Languages:
              <!-- <span class="opacity-70">{{ info.capital[0] }}</span> -->
            </li>
          </ul>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
import axios from "axios";

const data = ref(null);
const input = ref("");

const capitalizedInput = computed(() => {
  // Capitalize the first letter of the input
  return input.value.charAt(0).toUpperCase() + input.value.slice(1);
});

const getCountry = async () => {
  try {
    const response = await axios.get(
      `https://restcountries.com/v3.1/name/${capitalizedInput.value}`,
    );
    data.value = response.data;
    console.log(data.value);
  } catch (error) {
    console.error(error);
  }
};
</script>
