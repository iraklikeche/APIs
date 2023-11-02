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

      <div class="mt-8 max-w-lg" v-if="showFact">
        <div
          class="rounded-md border-b-8 border-yellow-400 px-8 py-10 shadow-2xl"
        >
          <h2 class="mb-4 text-4xl font-bold">{{ fact }}</h2>
          <p class="text-xl">{{ data }}</p>
          <p v-if="isEmpty" class="text-xl text-red-600">
            The input field cannot be empty. <br />
            Please choose the category.
          </p>
        </div>
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
const showFact = ref(false);
const isEmpty = ref(false);

watch(selectedOption, (newValue) => {
  console.log("Selected option:", newValue);
});

const fetchData = async (url) => {
  try {
    const response = await axios.get(url);
    data.value = response.data;
    console.log(data.value);
    showFact.value = true;
    if (url.includes("random")) {
      fact.value = "";
    }
  } catch (error) {
    console.error(error);
  }
};

const getFact = () => {
  if (!fact.value || !selectedOption.value) {
    isEmpty.value = true;
    showFact.value = true;
    data.value = "";
  } else {
    const url = `http://numbersapi.com/${fact.value}/${selectedOption.value}`;
    fetchData(url);
    isEmpty.value = false;
  }
};

const getRandomFact = () => {
  const url = "http://numbersapi.com/random";
  fetchData(url);
};
</script>
