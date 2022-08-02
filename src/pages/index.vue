<script setup>
import axios from 'axios'

const helloFromApi = ref('')

const apiRequest = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/hello`)
  helloFromApi.value = response.data
}

apiRequest()
</script>

<template>
  <div class="mt-10 text-red-500">
    {{ helloFromApi }}
  </div>
</template>

