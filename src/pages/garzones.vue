<script setup>
import axios from 'axios'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/outline/index.js'
import { onMounted } from 'vue'

const emit = defineEmits(['changedSelectedNavbarItem'])
const monthlySalesByWaiter = ref(undefined)
const totalSalesByWaiter = ref(undefined)

const getMonthlySalesByWaiter = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/monthly_sales_by_waiter`)
  monthlySalesByWaiter.value = response.data
}

const getTotalSalesByWaiter = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/total_sales_by_waiter`)
  totalSalesByWaiter.value = response.data
}

onMounted(() => {
  emit('changedSelectedNavbarItem', 'Garzones')
  getMonthlySalesByWaiter()
  getTotalSalesByWaiter()
})
</script>

<template>
  <div>
    <div class="max-w-7xl mx-auto py-12 px-4 sm:py-16 sm:px-6 lg:px-8">
      <section>
        <div class="max-w-3xl mx-auto divide-y-2 divide-gray-400">
          <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
            Estádisticas mensuales para garzones
          </h2>
          <dl class="mt-6 space-y-6 divide-y divide-gray-400">
            <Disclosure v-for="month in monthlySalesByWaiter" :key="month.date" v-slot="{ open }" as="div" class="pt-6">
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
                        Propina
                      </th>
                    </tr>
                  </thead>
                  <tbody class="divide-y divide-gray-200 bg-white">
                    <tr v-for="waiter in month.waiters_sales" :key="waiter.name">
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        {{ Number(waiter.name).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ Number(waiter.total_sales_count).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ Number(waiter.total_sales_income).toLocaleString() }}
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ Number(waiter.total_earned_tip).toLocaleString() }}
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
            Estádisticas generales para garzones
          </h2>
          <dl class="mt-6 space-y-6 divide-y divide-gray-400">
            <Disclosure v-for="waiter in totalSalesByWaiter" :key="waiter.name" v-slot="{ open }" as="div" class="pt-6">
              <dt class="text-lg">
                <DisclosureButton class="text-left w-full flex justify-between items-start text-gray-400">
                  <span class="font-medium text-gray-900">
                    {{ waiter.name }}
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
                        {{ Number(waiter.total_sales_count).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Ingresos por sus mesas
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ Number(waiter.total_sales_income).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Propina ganada
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        ${{ Number(waiter.total_earned_tip).toLocaleString() }}
                      </td>
                    </tr>
                    <tr>
                      <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                        Clientes atendidos
                      </td>
                      <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                        {{ Number(waiter.total_attended_clients).toLocaleString() }}
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
