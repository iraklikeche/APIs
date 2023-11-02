<template>
  <main class="bg- flex h-screen items-center justify-center">
    <div class="rounded-xl bg-white px-12 py-24">
      <div class="mb-8 flex items-center justify-center gap-4">
        <div>
          <input
            type="radio"
            id="math"
            name="category"
            value="math"
            v-model="selectedOption"
          />
          <label for="math">Math</label>
        </div>

        <div>
          <input
            type="radio"
            id="date"
            name="category"
            value="date"
            v-model="selectedOption"
          />
          <label for="date">Date</label>
        </div>
      </div>
      <div class="grid w-full grid-cols-[3fr_1fr] gap-4">
        <input
          type="number"
          min="0"
          class="border px-2"
          v-model="fact"
          placeholder="Get Fact About The Number"
        />
        <button
          class="rounded-md bg-yellow-400 px-8 py-2 text-white transition-opacity duration-500 hover:opacity-60"
          @click="getFact"
        >
          Get Fact
        </button>
      </div>
      <button
        class="mt-4 w-full rounded-md bg-yellow-400 px-4 py-2 font-bold text-white transition-opacity duration-500 hover:opacity-60"
        @click="getRandomFact"
      >
        Get Random Fact
      </button>

      <div class="mt-4 w-fit">
        <p>{{ data }}</p>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, watch } from "vue";
import axios from "axios";

const data = ref(null);
const fact = ref(null);
const selectedOption = ref(null);

watch(selectedOption, (newValue) => {
  console.log("Selected option:", newValue);
});

const getFact = async () => {
  try {
    const response = await axios.get(
      `http://numbersapi.com/${fact.value}/${selectedOption.value}`,
    );
    console.log(response);
    data.value = response.data;
    console.log(data.value);
  } catch (error) {
    console.error(error);
  }
};

const getRandomFact = async () => {
  try {
    const response = await axios.get("http://numbersapi.com/random");
    data.value = response.data;
    console.log(data.value);
    fact.value = "";
  } catch (error) {
    console.error(error);
  }
};
</script>
