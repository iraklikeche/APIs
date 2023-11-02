<template>
  <main class="flex items-center justify-center px-24 py-24">
    <div class="flex w-1/2 min-w-max flex-col rounded-xl bg-white px-10 py-12">
      <div class="flex items-center justify-center gap-8">
        <input
          placeholder="Type a Country"
          v-model="input"
          class="w-full rounded-lg border-b-2 border-[#3d64e6] bg-transparent p-2 px-4 placeholder-black"
        />
        <button
          @click="getCountry"
          class="rounded-full bg-yellow-400 px-8 py-2 text-center tracking-wider text-white transition-opacity duration-500 hover:opacity-50"
        >
          Search
        </button>
      </div>

      <div
        v-if="data"
        v-for="(singleCountry, index) in data"
        :key="index"
        class="py-8"
      >
        <div class="flex justify-center">
          <img :src="singleCountry.flags.png" class="shadow-2xl" />
        </div>
        <h1
          class="mt-8 text-center text-xl font-bold uppercase tracking-widest"
        >
          {{ singleCountry.name.official }}
        </h1>

        <div class="mt-8">
          <ul class="flex flex-col gap-2">
            <li class="font-bold">
              Capital:
              <span class="opacity-70" v-if="singleCountry.capital">{{
                singleCountry.capital.join(", ")
              }}</span>
              <span v-else>N/A</span>
            </li>
            <li class="font-bold">
              Population:
              <span class="opacity-70">{{ singleCountry.population }}</span>
            </li>
            <li class="font-bold">
              Currency:
              <span class="opacity-70">
                {{ Object.keys(singleCountry.currencies).join(", ") }}
              </span>
              &ndash;
              <span class="opacity-70">
                {{
                  singleCountry.currencies[
                    Object.keys(singleCountry.currencies)
                  ].name
                }}
              </span>
            </li>
            <li class="font-bold">
              Common Languages:
              <span class="opacity-70">{{
                Object.values(singleCountry.languages).join(", ")
              }}</span>
            </li>
            <li class="font-bold">
              Neighbors:
              <span class="opacity-70" v-if="singleCountry.borders">{{
                singleCountry.borders.join(", ")
              }}</span>
              <span class="opacity-70" v-else>None</span>
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

const input = ref("Georgia");

const capitalizedInput = computed(() => {
  // Capitalize the first letter of the input
  return input.value.charAt(0).toUpperCase() + input.value.slice(1);
});

const getCountry = async () => {
  try {
    const response = await axios.get(
      `https://restcountries.com/v3.1/name/${capitalizedInput.value}`,
    );

    const countryData = response.data;

    data.value = countryData;
    console.log(data.value);
  } catch (error) {
    console.error(error);
  }
};
</script>
