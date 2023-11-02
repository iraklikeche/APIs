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
        :to="{ name: 'facts' }"
        class="transition-all duration-500 hover:scale-110 hover:text-white"
        >Facts</RouterLink
      >
    </nav>
  </header>
</template>
