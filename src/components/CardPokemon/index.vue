<template>
  <article :class="('relative pt-5 pb-3 rounded-lg shadow flex flex-col items-center transition ease ring-2 ring-transparent hover:ring-emerald-500 hover:ring-offset-2 bg-opacity-20 bg-'+types[0].type.name)">
    <div class="absolute leading-none top-[2px] left-[2px] rounded-tl-lg rounded py-1 px-2 flex items-center bg-gray-100 bg-opacity-60">
      <span class="text-xs text-black">#{{ id }}</span>
    </div>
    <img
      :src="imageSource"
      :alt="alt"
      height="140"
      width="140"
      class="h-auto w-36"
    >
    <h2 class="mt-2 mb-1 text-center font-semibold capitalize">
      {{ name }}
    </h2>
    <ul class="flex items-center space-x-1">
      <li
        v-for="(value, index) in types"
        :key="'value'+index"
        :class="value.type.name + ' text-white rounded py-[2px] capitalize text-xs px-2'"
      >
        {{ value.type.name }}
      </li>
    </ul>
  </article>
</template>

<script>
export default {
  name: 'CardPokemon',
  props: {
    id: {
      type: [Number, String],
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    types: {
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
  computed: {
    imageSource() {
      return `${this.imageUrl}${this.id}.png`;
    },
    alt() {
      return `${this.name} image`;
    },
  },
};
</script>
