<template>
  <div class="max-w-lg min-h-screen mx-auto shadow-md">
    <div class="bg-[#FEFEFE]">
      <slot name="content" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'BaseLayout',
};
</script>
