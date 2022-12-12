<template>
  <BaseModal>
    <template #header>
      <div class="flex items-center justify-center h-10 w-10 rounded-full bg-emerald-100">
        <FilterIcon class="w-5 h-5" />
      </div>
      <h2 class="text-lg font-medium">
        Advanced Search
      </h2>
      <button
        type="button"
        class="absolute top-0 right-0 p-2"
        aria-label="Close modal"
        @click="close"
      >
        <XIcon class="w-4 h-4" />
      </button>
    </template>
    <template #body>
      <form @submit.prevent="onSubmit">
        <div class="pt-4">
          <div class="mt-2 space-y-3">
            <label
              for="pokemon_name"
              class="block flex-1 relative"
            >
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <SearchIcon class="w-5 h-5" />
              </div>
              <input
                id="pokemon_name"
                v-model="name"
                type="text"
                placeholder="Pokemon Name"
                aria-label="Search Pokemon Name"
                class="rounded-lg block text-gray-800 border w-full pl-10 py-2 pr-12"
              >
            </label>
            <p class="text-sm font-medium text-gray-700">
              Types
            </p>
            <div class="flex flex-wrap mt-3">
              <div
                v-for="type in types"
                :key="type.id"
              >
                <label
                  :for="type.name"
                  :class="'inline-flex items-center rounded-lg cursor-pointer mr-2 mb-2 pl-2 ring-1 ring-transparent transition ease border border-emerald-200 hover:ring-offset-2 bg-gray-700 hover:ring-emerald-200 '+type.name"
                >
                  <input
                    :id="type.name"
                    v-model="filterTypes"
                    type="checkbox"
                    :value="type.id"
                  >
                  <span class="px-4 py-[4px] text-sm leading-5 font-medium text-white capitalize"> {{ type.name }} </span>
                </label>
              </div>
            </div>
          </div>
        </div>
        <div class="mt-5">
          <button
            class="inline-flex justify-center w-full rounded-lg shadow-sm px-4 py-2 text-base font-medium text-white focus:outline-none focus:ring-2 focus:ring-offset-2 sm:text-sm bg-emerald-600 hover:bg-emerald-700 focus:ring-emerald-500"
            type="submit"
          >
            Search
          </button>
        </div>
      </form>
    </template>
  </BaseModal>
</template>

<script>
import BaseModal from '@/components/Modal/index.vue';
import { SearchIcon, FilterIcon, XIcon } from '@vue-hero-icons/outline';
import axios from 'axios';

export default {
  name: 'ModalFilter',
  components: {
    BaseModal,
    SearchIcon,
    FilterIcon,
    XIcon,
  },
  data() {
    return {
      name: '',
      types: [],
      filterTypes: [],
    };
  },
  mounted() {
    axios
      .get(`${process.env.VUE_APP_API_ENDPOINT}/type`)
      .then((response) => {
        response.data.results.forEach((type) => {
          type.id = type.url.split('/')
            .filter((part) => !!part).pop();
          this.types.push(type);
        });
      })
      .catch((error) => console.log(error)); /* eslint-disable-line no-console */
  },
  methods: {
    close() {
      this.$emit('close');
    },
    onSubmit() {
      this.$emit('inputFilter', this.name);
      this.$emit('close');
    },
  },
};
</script>
