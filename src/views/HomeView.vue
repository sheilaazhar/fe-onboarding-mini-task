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
      <div
        class="flex space-x-2 items-center justify-between px-6 py-4 bg-gray-50 bg-opacity-50 sticky top-0 z-10"
      >
        <label
          for="pokemon_name"
          class="block flex-1 relative"
        >
          <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
            <SearchIcon class="w-5 h-5" />
          </div>
          <input
            id="pokemon_name"
            v-model="search"
            type="text"
            name="search"
            placeholder="Pokemon Name"
            aria-label="Search Pokemon Name"
            class="pr-3 pl-10 py-2 bg-emerald-100 placeholder-gray-500 text-black rounded-lg border-none focus:ring-emerald-500 focus:ring-2 w-full"
            @keypress.enter="searchName"
          >
        </label>
        <button
          type="button"
          class="bg-gray-50 hover:bg-emerald-500 ring-2 ring-emerald-500 relative py-2 px-3 rounded-lg"
          @click="openFilter"
        >
          <FilterIcon class="w-5 h-5" />
          <div
            v-if="filterResult!== ''"
            class="w-2 h-2 rounded-full bg-blue-500 absolute top-[-2px] left-[-2px]"
          />
        </button>
      </div>
      <ModalFilter
        v-show="filterOpen"
        ref="formFilter"
        @close="closeFilter"
        @inputFilter="handleFilter"
      />
      <main class="px-6 pb-6">
        <p class="total-pokemon text-sm text-gray-500 text-right">
          Got {{ countData }} Pokemon
        </p>
        <LoaderIcon
          v-if="loader"
        />
        <section class="mt-3 grid grid-cols-2 gap-2">
          <div
            v-for="pokemon in dataPokemons"
            :key="pokemon.id"
          >
            <CardPokemon
              :id="pokemon.id"
              :name="pokemon.name"
              :types="pokemon.types"
            />
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
import CardPokemon from '@/components/CardPokemon/index.vue';
import LoaderIcon from '@/components/Loader/index.vue';

export default {
  name: 'HomeView',
  components: {
    BaseLayout,
    SearchIcon,
    FilterIcon,
    ModalFilter,
    CardPokemon,
    LoaderIcon,
  },
  data() {
    return {
      filterOpen: false,
      search: '',
      typeResult: [],
      filterResult: '',
      pokemons: [],
      allPokemons: [],
      nextUrl: '',
      currentUrl: '',
      loader: false,
      count: '',
    };
  },
  computed: {
    dataPokemons() {
      let dataPokemons = this.pokemons;
      if (this.search) {
        dataPokemons = this.allPokemons.filter((pokemon) => pokemon.name.toLowerCase().match(this.search.toLowerCase()));
      }
      if (this.filterResult) {
        dataPokemons = this.filterResult;
      }
      const orderedPokemons = dataPokemons.sort((a, b) => a.id - b.id);
      return orderedPokemons;
    },
    countData() {
      let countData = this.count;
      if (this.search || this.filterResult) {
        countData = this.dataPokemons.length;
      }
      return countData;
    },
  },
  created() {
    this.currentUrl = `${process.env.VUE_APP_API_ENDPOINT}/pokemon`;
    this.getPokemon();
    this.getAllPokemon();
  },
  mounted() {
    this.scrollTrigger();
  },
  methods: {
    getPokemon() {
      this.loader = true;
      axios
        .get(this.currentUrl)
        .then((response) => {
          this.nextUrl = response.data.next;
          this.count = response.data.count;
          response.data.results.forEach((pokemon) => {
            this.getPokemonType(pokemon);
          });
          this.loader = false;
        })
        .catch((error) => console.log(error)); /* eslint-disable-line no-console */
    },
    getPokemonType(pokemon) {
      axios
        .get(pokemon.url)
        .then((res) => {
          pokemon.types = res.data.types;
          pokemon.id = pokemon.url.split('/')
            .filter((part) => !!part).pop();
          this.pokemons.push(pokemon);
        })
        .catch((error) => console.log(error)); /* eslint-disable-line no-console */
    },
    getAllPokemon() {
      axios
        .get(`${process.env.VUE_APP_API_ENDPOINT}/pokemon?limit=1154&offset=0`)
        .then((response) => {
          response.data.results.forEach((pokemon) => {
            this.getAllPokemonType(pokemon);
          });
        })
        .catch((error) => console.log(error)); /* eslint-disable-line no-console */
    },
    getAllPokemonType(pokemon) {
      axios
        .get(pokemon.url)
        .then((res) => {
          pokemon.types = res.data.types;
          pokemon.id = pokemon.url.split('/')
            .filter((part) => !!part).pop();
          this.allPokemons.push(pokemon);
        })
        .catch((error) => console.log(error)); /* eslint-disable-line no-console */
    },
    openFilter() {
      this.filterOpen = true;
    },
    closeFilter() {
      this.filterOpen = false;
    },
    searchName() {
      this.filterResult = '';
      this.$refs.formFilter.name = '';
      this.$refs.formFilter.filterTypes = [];
    },
    async handleFilter(value) {
      this.typeResult = [];
      this.filterResult = '';
      if (value.name !== '' || value.types.length !== 0) {
        if (value.types.length === 0) {
          this.filterResult = this.allPokemons.filter((pokemon) => pokemon.name.toLowerCase().match(value.name.toLowerCase()));
        }
        if (value.types.length !== 0) {
          await Promise.all(
            value.types.map(async (type) => {
              await this.getFilterType(type);
            }),
          );
          this.filterResult = this.typeResult.filter((pokemon) => pokemon.name.toLowerCase().match(value.name.toLowerCase()));
        }
      } else {
        window.location.reload();
      }
    },
    async getFilterType(type) {
      try {
        this.loader = true;
        const response = await axios.get(`${process.env.VUE_APP_API_ENDPOINT}/type/${type}`);
        for (let i = 0; i < response.data.pokemon.length; i += 1) {
          this.typeResult[i] = {
            name: response.data.pokemon[i].pokemon.name,
            url: response.data.pokemon[i].pokemon.url,
            types: await this.getType(response.data.pokemon[i].pokemon.url), /* eslint-disable-line no-await-in-loop */
            id: response.data.pokemon[i].pokemon.url.split('/').filter((part) => !!part).pop(),
          };
        }
        this.loader = false;
      } catch (err) {
        console.log(err); /* eslint-disable-line no-console */
      }
    },
    async getType(pokeUrl) {
      const response = await axios.get(pokeUrl);
      return response.data.types;
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
      this.getPokemon();
    },
  },
};
</script>
