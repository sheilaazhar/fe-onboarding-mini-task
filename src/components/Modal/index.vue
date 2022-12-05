<template>
  <div class="fixed z-10 inset-0 overflow-y-auto bg-gray-500 backdrop-blur-sm bg-opacity-75 transition-opacity">
    <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center">
      <div class="relative inline-block align-bottom bg-white rounded-lg px-6 pt-5 pb-4 text-left overflow-hidden shadow-xl transition-all">
        <div class="flex items-center space-x-4">
          <slot name="header" />
        </div>
        <slot name="body" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BaseModal',
};
</script>
