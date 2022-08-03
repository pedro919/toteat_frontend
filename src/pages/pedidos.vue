<script setup>
import axios from 'axios'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/outline/index.js'
import { onMounted } from 'vue'

const emit = defineEmits(['changedSelectedNavbarItem'])
const allOrders = ref(undefined)

const getAllOrders = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/all_orders`)
  allOrders.value = response.data
}

onMounted(async () => {
  emit('changedSelectedNavbarItem', 'Pedidos')
  await getAllOrders()
})
</script>

<template>
  <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
    Todos los pedidos
  </h2>
  <div v-if="allOrders === undefined" class="mt-10 text-center text-2xl text-gray-900">
    Cargando datos...
  </div>
  <dl class="mt-6 space-y-6 divide-y divide-gray-400">
    <Disclosure v-for="order in allOrders" :key="order.id" v-slot="{ open }" as="div" class="pt-6">
      <dt class="text-lg">
        <DisclosureButton class="text-left w-full flex justify-between items-start text-gray-400">
          <span class="font-medium text-gray-900">
            {{ order.date_opened }}
          </span>
          <span class="ml-6 h-7 flex items-center">
            <ChevronDownIcon class="h-6 w-6 transform" :class="[open ? '-rotate-180' : 'rotate-0']" aria-hidden="true" />
          </span>
        </DisclosureButton>
      </dt>
      <DisclosurePanel as="dd" class="mt-2 pr-12">
        <table class="min-w-full divide-y divide-gray-300 border-2 border-gray-300 mt-5">
          <tbody class="divide-y divide-gray-200 bg-white">
            <tr>
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                Venta total
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                ${{ order.total }}
              </td>
            </tr>
            <tr>
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                Hora de salida
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ order.date_closed }}
              </td>
            </tr>
            <tr>
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                Clientes
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ order.diners }}
              </td>
            </tr>
            <tr>
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                Mesero
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ order.waiter }}
              </td>
            </tr>
            <tr>
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                Cajero
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ order.cashier }}
              </td>
            </tr>
            <tr>
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                Zona
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ order.zone }}
              </td>
            </tr>
          </tbody>
        </table>

        <div class="mt-10">
          <table class="min-w-full divide-y divide-gray-300">
            <thead class="bg-gray-50">
              <tr>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                  Plato
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                  Categoria
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                  Cantidad
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center w-1/3">
                  Precio
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 bg-white">
              <tr v-for="product in order.products" :key="product.name">
                <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6 w-1/3">
                  {{ product.name }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                  {{ product.category }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                  {{ product.quantity }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                  ${{ product.price }}
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
                  Cantidad pagada
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 bg-white">
              <tr v-for="payment in order.payments" :key="payment.name">
                <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6 w-1/3">
                  {{ payment.type }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 w-1/3">
                  ${{ payment.amount }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </DisclosurePanel>
    </Disclosure>
  </dl>
</template>
