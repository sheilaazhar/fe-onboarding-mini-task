<template>
  <article class="relative pt-5 pb-3 rounded-lg shadow flex flex-col items-center transition ease ring-2 ring-transparent hover:ring-emerald-500 hover:ring-offset-2 bg-green-700 bg-opacity-20">
    <div class="absolute leading-none top-[2px] left-[2px] rounded-tl-lg rounded py-1 px-2 flex items-center bg-green-500 bg-opacity-60">
      <span class="text-xs text-black">#{{ pokemon.id }}</span>
    </div>
    <img
      :src="imageUrl + pokemon.id + '.png'"
      :alt="pokemon.name + ' image'"
      height="140"
      width="140"
      class="h-auto w-36"
    >
    <h2 class="mt-2 text-center font-semibold capitalize">
      {{ pokemon.name }}
    </h2>
  </article>
</template>

<script>
export default {
  name: 'CardPokemon',
  props: {
    pokemon: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      imageUrl:
        'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/',
    };
  },
};
</script>
