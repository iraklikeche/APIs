<script setup>
import axios from "axios";

const getData = async () => {
  const response = await axios.get("https://v2.jokeapi.dev/joke/Any");
  console.log(response);
  const data = response.data;
  console.log(data);
};

getData();
</script>

<template>
  <main>
    <input type="text" />
    <input type="text" />
    <input type="text" />
    <input type="text" />
    <input type="text" />
  </main>
</template>
