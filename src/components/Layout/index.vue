<template>
  <div class="max-w-lg min-h-screen mx-auto shadow-md bg-gray-50">
    <slot name="content" />
  </div>
</template>

<script>
export default {
  name: 'BaseLayout',
};
</script>
