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
    </template>
  </BaseLayout>
</template>

<script>
import BaseLayout from '@/components/Layout/index.vue';

export default {
  name: 'HomeView',
  components: {
    BaseLayout,
  },
};
</script>
