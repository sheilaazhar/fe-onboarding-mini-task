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
            <!-- List of types still in progress -->
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
    };
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
