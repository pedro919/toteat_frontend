<script setup>
import axios from 'axios'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/outline/index.js'
import { onMounted } from 'vue'

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
  <div>
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
                        Ingresos totales
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ Number(month.data.total_income).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Clientes totales
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ Number(month.data.total_clients).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Ingreso por cliente
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ Number(month.data.income_per_client).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Tiempo de permanencia
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ Number(month.data.average_time_in_restaurant).toLocaleString() }} min
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
                        Forma de pago
                      </th>
                      <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                        Mesas
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
                        {{ Number(payment.count).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                        ${{ Number(payment.income).toLocaleString() }}
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
                        Mesa
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
                        {{ Number(zone.count).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                        ${{ Number(zone.income).toLocaleString() }}
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
  </div>
</template>
