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
      <form class="flex space-x-2 items-center justify-between px-6 py-4 bg-gray-50 bg-opacity-50 sticky top-0 z-10">
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
          class="bg-gray-50 hover:bg-emerald-500 ring-2 ring-emerald-500 py-2 px-3 rounded-lg"
          @click="openFilter"
        >
          <FilterIcon class="w-5 h-5" />
        </button>
      </form>
      <ModalFilter
        v-show="filterOpen"
        @close="closeFilter"
        @inputFilter="handleFilter"
      />
      <main class="px-6 pb-6">
        <p class="total-pokemon text-sm text-gray-500 text-right">
          Got {{ countData }} Pokemon
        </p>
        <section class="mt-3 grid grid-cols-2 gap-2">
          <div
            v-for="pokemon in pokemons"
            :key="pokemon.id"
          >
            <CardPokemon :pokemon="pokemon" />
          </div>
          <div
            id="scroll-trigger"
            ref="infinitescrolltrigger"
          />
        </section>
      </main>
    </template>
  </BaseLayout>
</template>

<script>
import BaseLayout from '@/components/Layout/index.vue';
import { SearchIcon, FilterIcon } from '@vue-hero-icons/outline';
import ModalFilter from '@/components/Filter/index.vue';
import axios from 'axios';
import CardPokemon from '@/components/Card Pokemon/index.vue';

export default {
  name: 'HomeView',
  components: {
    BaseLayout,
    SearchIcon,
    FilterIcon,
    ModalFilter,
    CardPokemon,
  },
  data() {
    return {
      filterOpen: false,
      searchName: '',
      pokemons: [],
      apiUrl: 'https://pokeapi.co/api/v2/pokemon',
      nextUrl: '',
      currentUrl: '',
      countData: '',
    };
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  },
  methods: {
    fetchData() {
      axios
        .get(this.currentUrl)
        .then((response) => {
          this.nextUrl = response.data.next;
          this.countData = response.data.count;
          response.data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url.split('/')
              .filter((part) => !!part).pop();
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => console.log(error)); /* eslint-disable-line no-console */
    },
    openFilter() {
      this.filterOpen = true;
    },
    closeFilter() {
      this.filterOpen = false;
    },
    handleFilter(value) {
      this.searchName = value;
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });
      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
  },
};
</script>
