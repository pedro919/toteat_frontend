<script setup>
import axios from 'axios'
import { CreditCardIcon } from '@heroicons/vue/solid/index.js'
import { ClockIcon, UsersIcon } from '@heroicons/vue/outline/index.js'
import { onMounted } from 'vue'

const emit = defineEmits(['changedSelectedNavbarItem'])
const totalSales = ref(undefined)
const getTotalSales = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/total_sales`)
  totalSales.value = response.data
}

onMounted(async () => {
  emit('changedSelectedNavbarItem', 'Inicio')
  await getTotalSales()
})
</script>

<template>
  <div>
    <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
      La pikada de la esquina
    </h2>

    <dl class="mt-10 grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-3 mx-4 md:mx-10">
      <div class="relative bg-white pt-5 px-4 pb-6 sm:pt-6 sm:px-6 shadow-xl rounded-lg overflow-hidden">
        <dt>
          <div class="absolute bg-indigo-500 rounded-md p-3">
            <component :is="UsersIcon" class="h-6 w-6 text-white" aria-hidden="true" />
          </div>
          <p class="ml-16 text-md font-medium text-gray-500 truncate text-left">
            Clientes totales
          </p>
        </dt>
        <dd class="ml-16 pb-6 flex items-baseline sm:pb-7">
          <p v-if="totalSales !== undefined" class="text-2xl font-semibold text-gray-900">
            {{ totalSales.total_clients }}
          </p>
        </dd>
      </div>
      <div class="relative bg-white pt-5 px-4 pb-6 sm:pt-6 sm:px-6 shadow-xl rounded-lg overflow-hidden">
        <dt>
          <div class="absolute bg-indigo-500 rounded-md p-3">
            <component :is="CreditCardIcon" class="h-6 w-6 text-white" aria-hidden="true" />
          </div>
          <p class="ml-16 text-md font-medium text-gray-500 truncate text-left">
            Ingresos totales
          </p>
        </dt>
        <dd class="ml-16 pb-6 flex items-baseline sm:pb-7">
          <p v-if="totalSales !== undefined" class="text-2xl font-semibold text-gray-900">
            ${{ totalSales.total_income }}
          </p>
        </dd>
      </div>
      <div class="relative bg-white pt-5 px-4 pb-6 sm:pt-6 sm:px-6 shadow-xl rounded-lg overflow-hidden">
        <dt>
          <div class="absolute bg-indigo-500 rounded-md p-3">
            <component :is="ClockIcon" class="h-6 w-6 text-white" aria-hidden="true" />
          </div>
          <p class="ml-16 text-md font-medium text-gray-500 truncate text-left">
            Tiempo permanencia (min)
          </p>
        </dt>
        <dd class="ml-16 pb-6 flex items-baseline sm:pb-7">
          <p v-if="totalSales !== undefined" class="text-2xl font-semibold text-gray-900">
            {{ totalSales.average_time_in_restaurant }}
          </p>
        </dd>
      </div>
    </dl>

    <div v-if="totalSales !== undefined" class="mt-10 mx-10">
      <table class="min-w-full divide-y divide-gray-300">
        <thead class="bg-gray-50">
          <tr>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
              Zona
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
              Mesa
            </th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
              Ingresos
            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200 bg-white">
          <tr v-for="zone in totalSales.zones" :key="zone.name">
            <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6 w-1/3">
              {{ zone.name }}
            </td>
            <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
              {{ zone.count }}
            </td>
            <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
              ${{ zone.income }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
