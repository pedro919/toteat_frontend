<script setup>
import axios from 'axios'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/outline/index.js'
import { onMounted } from 'vue'

const emit = defineEmits(['changedSelectedNavbarItem'])
const monthlySalesByCashier = ref(undefined)
const totalSalesByCashier = ref(undefined)

const getMonthlySalesByCashier = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/monthly_sales_by_cashier`)
  monthlySalesByCashier.value = response.data
}

const getTotalSalesByCashier = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/total_sales_by_cashier`)
  totalSalesByCashier.value = response.data
}

onMounted(() => {
  emit('changedSelectedNavbarItem', 'Cajeros')
  getMonthlySalesByCashier()
  getTotalSalesByCashier()
})
</script>

<template>
  <div>
    <div class="max-w-7xl mx-auto py-12 px-4 sm:py-16 sm:px-6 lg:px-8">
      <section>
        <div class="max-w-3xl mx-auto divide-y-2 divide-gray-400">
          <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
            Estádisticas mensuales para cajeros
          </h2>
          <dl class="mt-6 space-y-6 divide-y divide-gray-400">
            <Disclosure v-for="month in monthlySalesByCashier" :key="month.date" v-slot="{ open }" as="div" class="pt-6">
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
                <table class="min-w-full divide-y divide-gray-300">
                  <thead class="bg-gray-50">
                    <tr>
                      <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6">
                        Nombre
                      </th>
                      <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                        Mesas
                      </th>
                      <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                        Ingresos por sus mesas
                      </th>
                      <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                        Clientes atendidos
                      </th>
                    </tr>
                  </thead>
                  <tbody class="divide-y divide-gray-200 bg-white">
                    <tr v-for="cashier in month.cashiers_sales" :key="cashier.name">
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        {{ cashier.name }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ (cashier.total_sales_count).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ (cashier.total_sales_income).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ (cashier.total_attended_clients).toLocaleString() }}
                      </td>
                    </tr>
                  </tbody>
                </table>
              </DisclosurePanel>
            </Disclosure>
          </dl>
        </div>
      </section>

      <section class="mt-28 lg.mt-56">
        <div class="max-w-3xl mx-auto divide-y-2 divide-gray-400">
          <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
            Estádisticas generales para cajeros
          </h2>
          <dl class="mt-6 space-y-6 divide-y divide-gray-400">
            <Disclosure v-for="cashier in totalSalesByCashier" :key="cashier.name" v-slot="{ open }" as="div" class="pt-6">
              <dt class="text-lg">
                <DisclosureButton class="text-left w-full flex justify-between items-start text-gray-400">
                  <span class="font-medium text-gray-900">
                    {{ cashier.name }}
                  </span>
                  <span class="ml-6 h-7 flex items-center">
                    <ChevronDownIcon class="h-6 w-6 transform" :class="[open ? '-rotate-180' : 'rotate-0']" aria-hidden="true" />
                  </span>
                </DisclosureButton>
              </dt>
              <DisclosurePanel as="dd" class="mt-2 pr-12">
                <table class="min-w-full divide-y divide-gray-300">
                  <tbody class="divide-y divide-gray-200 bg-white">
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Mesas
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ Number(cashier.total_sales_count).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Ingresos por sus mesas
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ (cashier.total_sales_income).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Clientes atendidos
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ (cashier.total_attended_clients).toLocaleString() }}
                      </td>
                    </tr>
                  </tbody>
                </table>
              </DisclosurePanel>
            </Disclosure>
          </dl>
        </div>
      </section>
    </div>
  </div>
</template>
