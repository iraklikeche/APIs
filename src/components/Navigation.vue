<template>
  <header>
    <nav class="flex items-center justify-around pt-6 text-xl font-bold">
      <RouterLink
        to="/"
        class="transition-all duration-500 hover:scale-110 hover:text-white"
        >Jokes</RouterLink
      >
      <RouterLink
        :to="{ name: 'countries' }"
        class="transition-all duration-500 hover:scale-110 hover:text-white"
        >Countries</RouterLink
      >
      <RouterLink
        to="/countries"
        class="transition-transition-all duration-500 hover:scale-110 hover:text-white"
        >Countries</RouterLink
      >
      <RouterLink
        to="/countries"
        class="transition-transition-all duration-500 hover:scale-110 hover:text-white"
        >Countries</RouterLink
      >
      <RouterLink
        to="/countries"
        class="transition-transition-all duration-500 hover:scale-110 hover:text-white"
        >Countries</RouterLink
      >
    </nav>
  </header>
</template>
