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

const categories = [
  { value: "Any", label: "Any" },
  { value: "programming", label: "Programming" },
  { value: "Misc", label: "Misc" },
  { value: "Dark", label: "Dark" },
  { value: "Pun", label: "Pun" },
  { value: "Spooky", label: "Spooky" },
  { value: "Christmas", label: "Christmas" },
];

const languages = [
  { value: "", label: "English" },
  { value: "es", label: "Spanish" },
  { value: "de", label: "German" },
  { value: "pt", label: "Portuguese" },
  { value: "cs", label: "Czech" },
  { value: "fr", label: "French" },
];

const apiUrl = computed(() => {
  return `https://v2.jokeapi.dev/joke/${selectedOption.value}?lang=${selectedLanguage.value}&type=${selectedParts.value}&amount=${numberOfJokes.value}`;
});

watch(selectedOption, (newValue) => {
  console.log("Selected option:", newValue);
});
console.log(joke.value);

const getData = async () => {
  try {
    const response = await axios.get(apiUrl.value);
    const data = response.data;
    joke.value = data;
    console.log(data);
  } catch (error) {
    console.error("Error fetching data:", error);
  }
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
  <main class="grid grid-cols-1 items-center gap-12 px-24 pt-24 lg:grid-cols-2">
    <form @submit.prevent="getData">
      <!-- CATEGORY -->
      <div class="rounded-lg border border-black p-12">
        <div class="mb-4 w-full">
          <span>Select Category:</span>
          <div class="mt-2 flex rounded-lg border-2 border-black p-2">
            <select
              id="category"
              name="category"
              v-model="selectedOption"
              class="w-full bg-transparent focus:outline-none"
            >
              <option v-for="category in categories" :key="category.value">
                {{ category.label }}
              </option>
            </select>
          </div>

          <!-- LANGUAGE  -->
          <div class="mb-4">
            <span>Select Language:{{ selectedLanguage }}</span>
            <div class="mt-2 rounded-lg border-2 border-black p-2">
              <select
                id="language"
                name="languages"
                class="w-full bg-transparent focus:outline-none"
                v-model="selectedLanguage"
              >
                <option
                  v-for="language in languages"
                  :key="language.value"
                  :value="language.value"
                >
                  {{ language.label }}
                </option>
              </select>
            </div>
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
              min="1"
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
        <div>
          <h1
            class="mb-8 mt-6 text-center text-3xl font-bold"
            v-if="!jokes.error"
          >
            {{ jokes.category }} Joke
          </h1>
          <h1 v-else class="mb-8 mt-6 text-center text-3xl font-bold">
            {{ jokes.message }}
          </h1>
        </div>
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
        <div>
          <h1
            class="mb-8 mt-6 text-center text-3xl font-bold"
            v-if="!joke.error"
          >
            {{ joke.category }} Joke
          </h1>
          <h1 v-else class="mb-8 mt-6 text-center text-3xl font-bold">
            {{ joke.message }}
          </h1>
        </div>
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
