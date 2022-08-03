<script setup>
import axios from 'axios'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/outline/index.js'

const emit = defineEmits(['changedSelectedNavbarItem'])
const monthlyReport = ref(undefined)

const getMonthlyReport = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/monthly_sales`)
  monthlyReport.value = response.data
}

onMounted(() => {
  emit('changedSelectedNavbarItem', 'Reporte mensual')
  getMonthlyReport()
})
</script>

<template>
  <section class="mt-10">
    <div class="max-w-3xl mx-auto divide-y-2 divide-gray-400">
      <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
        Reportes de operaciones por mes
      </h2>
      <dl class="mt-6 space-y-6 divide-y divide-gray-400">
        <Disclosure v-for="month in monthlyReport" :key="month.date" v-slot="{ open }" as="div" class="pt-6">
          <dt class="text-lg">
            <DisclosureButton class="text-left w-full flex justify-between items-start text-gray-400">
              <span class="font-medium text-gray-900">
                {{ month.date }}
              </span>
              <span class="ml-6 h-7 flex items-center">
                <ChevronDownIcon class="h-6 w-6 transform" :class="[open ? '-rotate-180' : 'rotate-0']" aria-hidden="true" />
              </span>
            </DisclosureButton>
          </dt>
          <DisclosurePanel as="dd" class="mt-2 pr-12">
            <div>
              <h1>Resumen general</h1>
              <table class="min-w-full divide-y divide-gray-300 border-2 border-gray-300 mt-5">
                <tbody class="divide-y divide-gray-200 bg-white">
                  <tr>
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                      Ventas totales
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                      {{ month.data.total_income }}
                    </td>
                  </tr>
                  <tr>
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                      Clientes totales
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                      {{ month.data.total_clients }}
                    </td>
                  </tr>
                  <tr>
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                      Ingreso por cliente
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                      {{ month.data.total_income }}
                    </td>
                  </tr>
                  <tr>
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                      Tiempo de permanencia promedio
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                      {{ month.data.average_time_in_restaurant }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="mt-10">
              <table class="min-w-full divide-y divide-gray-300">
                <thead class="bg-gray-50">
                  <tr>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                      Formma de pago
                    </th>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                      Uso
                    </th>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                      Ingresos por este medio
                    </th>
                  </tr>
                </thead>
                <tbody class="divide-y divide-gray-200 bg-white">
                  <tr v-for="payment in month.data.payments" :key="payment.name">
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6 w-1/3">
                      {{ payment.name }}
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                      {{ payment.count }}
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                      {{ payment.income }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="mt-10">
              <table class="min-w-full divide-y divide-gray-300">
                <thead class="bg-gray-50">
                  <tr>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                      Zona
                    </th>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                      Uso
                    </th>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                      Ingresos
                    </th>
                  </tr>
                </thead>
                <tbody class="divide-y divide-gray-200 bg-white">
                  <tr v-for="zone in month.data.zones" :key="zone.name">
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6 w-1/3">
                      {{ zone.name }}
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                      {{ zone.count }}
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                      {{ zone.income }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </DisclosurePanel>
        </Disclosure>
      </dl>
    </div>
  </section>
</template>
