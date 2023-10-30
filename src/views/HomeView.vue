<script setup>
import { ref, computed, watch, onMounted } from "vue";
import axios from "axios";

const selectedOption = ref("Any");
const selectedLanguage = ref("");
const selectedParts = ref("");
const singlePartSelected = ref(false);
const twoPartSelected = ref(false);
const numberOfJokes = ref(1);
const joke = ref([]);

const apiUrl = computed(() => {
  return `https://v2.jokeapi.dev/joke/${selectedOption.value}?lang=${selectedLanguage.value}&type=${selectedParts.value}&amount=${numberOfJokes.value}`;
});

watch(selectedOption, (newValue) => {
  console.log("Selected option:", newValue);
});

watch(joke, (newValue) => {
  console.log(joke.value);
});

const getData = async () => {
  try {
    const response = await axios.get(apiUrl.value);
    const data = response.data;
    joke.value = data;
    // console.log(data);
  } catch (error) {
    console.error("Error fetching data:", error);
  }
  // console.log(apiUrl.value);
};

watch([singlePartSelected, twoPartSelected], () => {
  if (singlePartSelected.value && twoPartSelected.value) {
    selectedParts.value = ""; // Both selected
  } else if (singlePartSelected.value) {
    selectedParts.value = "single"; // Only single selected
  } else if (twoPartSelected.value) {
    selectedParts.value = "twopart"; // Only twopart selected
  } else {
    selectedParts.value = ""; // None selected
  }
});

onMounted(() => {
  getData(); // Call getData() when the component is mounted (page load or refresh)
});
</script>

<template>
  <main class="grid grid-cols-2 items-center gap-12 px-24">
    <form
      class="flex h-screen items-center justify-center"
      @submit.prevent="getData"
    >
      <!-- CATEGORY -->
      <div class="rounded-lg border border-black p-12">
        <div class="mb-4 w-full">
          <span>Select Category:</span>
          <div class="mt-2 rounded-lg border-2 border-black p-2">
            <input
              type="radio"
              id="Any"
              name="category"
              value="Any"
              v-model="selectedOption"
            />
            <label for="Any" class="mr-8">Any</label>

            <input
              type="radio"
              id="programming"
              name="category"
              value="programming"
              v-model="selectedOption"
              class="mr-[2px]"
            />
            <label class="mr-4">Programming</label>

            <input
              type="radio"
              id="Misc"
              name="category"
              value="Misc"
              v-model="selectedOption"
              class="mr-[2px]"
            />
            <label class="mr-4">Misc</label>

            <input
              type="radio"
              id="Dark"
              name="category"
              value="Dark"
              v-model="selectedOption"
              class="mr-[2px]"
            />
            <label class="mr-4">Dark</label>

            <input
              type="radio"
              id="Pun"
              name="category"
              value="Pun"
              v-model="selectedOption"
              class="mr-[2px]"
            />
            <label class="mr-4">Pun</label>

            <input
              type="radio"
              id="Spooky"
              name="category"
              value="Spooky"
              v-model="selectedOption"
              class="mr-[2px]"
            />
            <label class="mr-4">Spooky</label>

            <input
              type="radio"
              id="Christmas"
              name="category"
              value="Christmas"
              v-model="selectedOption"
              class="mr-[2px]"
            />
            <label>Christmas</label>
          </div>
        </div>

        <!-- LANGUAGE  -->
        <div class="mb-4">
          <span>Select Language:{{ selectedLanguage }}</span>
          <div class="mt-2 rounded-lg border-2 border-black p-2">
            <select
              id="cars"
              name="cars"
              class="w-full bg-transparent"
              v-model="selectedLanguage"
            >
              <option value="">English</option>
              <option value="es">Spanish</option>
              <option value="de">German</option>
              <option value="pt">Portuguese</option>
              <option value="cs">Czech</option>
              <option value="fr">French</option>
            </select>
          </div>
        </div>

        <!-- SINGLE / TWOPARTS -->
        <div class="mb-4 w-full">
          <span>Select at least one joke type: {{ selectedParts }}</span>
          <div class="mt-2 rounded-lg border-2 border-black p-2">
            <input
              type="checkbox"
              class="mr-[2px]"
              v-model="singlePartSelected"
            />
            <label class="mr-8">Single</label>

            <input type="checkbox" class="mr-[2px]" v-model="twoPartSelected" />
            <label>Two-part</label>
          </div>
        </div>

        <div class="w-full">
          <span>Amount of jokes:</span>
          <div class="mt-2 rounded-lg border-2 border-black p-2">
            <input
              type="number"
              v-model="numberOfJokes"
              class="w-full bg-transparent focus:outline-none"
            />
          </div>
        </div>

        <button
          class="mt-4 w-full bg-black py-4 text-2xl text-white transition-colors hover:bg-yellow-400 hover:text-black hover:shadow-2xl"
        >
          Generate Joke!
        </button>
      </div>
    </form>

    <div class="w-full rounded-lg border border-black p-12">
      <div v-if="joke.jokes" v-for="jokes in joke.jokes">
        <h1 class="mb-2 mt-6 text-center text-3xl font-bold">
          {{ jokes.category }} Joke
        </h1>
        <span v-if="jokes.type === 'single'" class="text-xl">{{
          jokes.joke
        }}</span>
        <div v-else>
          <span class="text-xl">&ndash; {{ jokes.setup }}</span>
          <br />
          <span class="text-xl">&ndash; {{ jokes.delivery }}</span>
        </div>
      </div>

      <div v-else>
        <h1 class="my-4 text-center text-3xl font-bold">
          {{ joke.category }} Joke
        </h1>
        <span v-if="joke.type === 'single'" class="text-xl">{{
          joke.joke
        }}</span>
        <div v-else>
          <span class="text-xl">&ndash; {{ joke.setup }}</span>
          <br />
          <span class="text-xl">&ndash; {{ joke.delivery }}</span>
        </div>
      </div>
    </div>
  </main>
</template>
