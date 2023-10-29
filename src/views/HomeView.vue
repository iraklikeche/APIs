<script setup>
import { ref, computed, watch } from "vue";
import axios from "axios";

const selectedOption = ref("Any");
const selectedLanguage = ref("");
const selectedParts = ref({
  single: "",
  twopart: "",
});
console.log(selectedParts.value);

const apiUrl = computed(() => {
  return `https://v2.jokeapi.dev/joke/${selectedOption.value}?lang=${selectedLanguage.value}&type=${selectedParts.value}`;
});

watch(selectedOption, (newValue) => {
  console.log("Selected option:", newValue);
});

const getData = async () => {
  try {
    const response = await axios.get(apiUrl.value);
    console.log(response);
    const data = response.data;
    console.log(data);
  } catch (error) {
    console.error("Error fetching data:", error);
  }
  console.log(apiUrl.value);
};
</script>

<template>
  <form
    class="flex h-screen items-center justify-center bg-yellow-400"
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
            v-model="selectedParts.single"
          />
          <label class="mr-8">Single</label>

          <input
            type="checkbox"
            class="mr-[2px]"
            v-model="selectedParts.twopart"
          />
          <label>Two-part</label>
        </div>
      </div>

      <div class="w-full">
        <span>Amount of jokes:</span>
        <div class="mt-2 rounded-lg border-2 border-black p-2">
          <input
            type="number"
            class="w-full bg-transparent focus:outline-none"
          />
        </div>
      </div>

      <button
        @click="getData()"
        class="mt-4 w-full bg-black py-4 text-2xl text-white transition-colors hover:bg-yellow-400 hover:text-black hover:shadow-2xl"
      >
        Generate Joke!
      </button>
    </div>
  </form>
</template>
