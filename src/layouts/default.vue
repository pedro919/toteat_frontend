<script setup>
const emit = defineEmits(['changedSelectedNavbarItem'])
const handler = (name) => {
  emit('changedSelectedNavbarItem', name)
}
</script>

<template>
  <main class="px-4 py-10 text-center text-gray-700 dark:text-gray-200">
    <RouterView
      @changedSelectedNavbarItem="handler"
    />
  </main>
</template>
