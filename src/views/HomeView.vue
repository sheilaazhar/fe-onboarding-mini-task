<template>
  <BaseLayout>
    <template #content>
      <header class="px-6 pt-12">
        <h1 class="mb-2 font-semibold text-4xl">
          Pokedex
        </h1>
        <p class="text-md">
          Find your favorite Pokemon!
        </p>
      </header>
      <form class="flex space-x-2 items-center justify-between px-6 py-4">
        <label
          for="pokemon_name"
          class="block flex-1 relative"
        >
          <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
            <SearchIcon class="w-5 h-5" />
          </div>
          <input
            id="pokemon_name"
            type="text"
            name="search"
            placeholder="Pokemon Name"
            aria-label="Search Pokemon Name"
            class="pr-3 pl-10 py-2 bg-emerald-100 placeholder-gray-500 text-black rounded-lg border-none focus:ring-emerald-500 focus:ring-2 w-full"
          >
        </label>
        <button
          type="button"
          class="bg-white hover:bg-emerald-500 ring-2 ring-emerald-500 py-2 px-3 rounded-lg"
        >
          <FilterIcon class="w-5 h-5" />
        </button>
      </form>
    </template>
  </BaseLayout>
</template>

<script>
import BaseLayout from '@/components/Layout/index.vue';
import { SearchIcon, FilterIcon } from '@vue-hero-icons/outline';

export default {
  name: 'HomeView',
  components: {
    BaseLayout,
    SearchIcon,
    FilterIcon,
  },
};
</script>
