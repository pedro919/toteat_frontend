<script setup lang="ts">
// https://github.com/vueuse/head
// you can use this to manipulate the document head in any components,
// they will be rendered correctly in the html results with vite-ssg
useHead({
  title: 'Toteat',
  meta: [
    { name: 'description', content: 'La pikada de la esquina' },
  ],
})

const navigation = ref([
  { name: 'Inicio', to: '/', current: true },
  { name: 'Reporte mensual', to: '/reportes', current: false },
  { name: 'Productos', to: '/productos', current: false },
  { name: 'Garzones', to: '/garzones', current: false },
  { name: 'Cajeros', to: '/cajeros', current: false },
  { name: 'Pedidos', to: '/pedidos', current: false },
])

const changeNavigationItemHandler = (event, name) => {
  const nextActiveItem = navigation.value.find(el => el.name === name)
  const currentActiveItem = navigation.value.find(el => el.current)
  if (nextActiveItem.name !== currentActiveItem.name) {
    nextActiveItem.current = true
    currentActiveItem.current = false
  }
}

const changeNavigationItemHandlerOnMounted = (name) => {
  const nextActiveItem = navigation.value.find(el => el.name === name)
  const currentActiveItem = navigation.value.find(el => el.current)
  if (nextActiveItem.name !== currentActiveItem.name) {
    nextActiveItem.current = true
    currentActiveItem.current = false
  }
}
</script>

<template>
  <TheNavbar
    :navigation="navigation"
    @changedSelectedNavbarItem="changeNavigationItemHandler"
  />
  <RouterView
    @changedSelectedNavbarItem="changeNavigationItemHandlerOnMounted"
  />
</template>
