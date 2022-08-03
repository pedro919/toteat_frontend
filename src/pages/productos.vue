<script setup>
import axios from 'axios'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/outline/index.js'
import { onMounted } from 'vue'

const emit = defineEmits(['changedSelectedNavbarItem'])
const productsSales = ref(undefined)
const monthlyProductsSales = ref(undefined)
const selectedCategory = ref(undefined)
const selectedCategoriesOnMonthlyReport = ref({})

const getProductsSales = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/products_sales`)
  productsSales.value = response.data
}

const getMonthlyProductsSales = async () => {
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/monthly_sales_by_product`)
  monthlyProductsSales.value = response.data
}

onMounted(async () => {
  emit('changedSelectedNavbarItem', 'Productos')
  await getProductsSales()
  await getMonthlyProductsSales()
  const dictionary = {}
  for (let i = 0; i < monthlyProductsSales.value.length; i++)
    dictionary[monthlyProductsSales.value[i].date] = undefined

  selectedCategoriesOnMonthlyReport.value = dictionary
})
</script>

<template>
  <div>
    <section class="max-w-7xl mx-auto py-12 px-4 sm:px-6 lg:py-16 lg:px-8">
      <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
        Desglose mensual de productos
      </h2>
      <dl class="mt-6 space-y-6 divide-y divide-gray-400">
        <Disclosure v-for="month in monthlyProductsSales" :key="month.date" v-slot="{ open }" as="div" class="pt-6">
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
            <div v-if="monthlyProductsSales !== undefined" class="mt-6 grid grid-cols-2 gap-0.5  lg:mt-8">
              <div v-for="category in month.monthly_sales.all_products" :key="category.name" class="col-span-1 flex justify-center py-8 px-8 cursor-pointer" :class="{ 'bg-gray-300': selectedCategoriesOnMonthlyReport[month.date] === category, 'bg-gray-200': selectedCategoriesOnMonthlyReport[month.date] !== category }" @click="selectedCategoriesOnMonthlyReport[month.date] = category">
                <h3>{{ category.name }}</h3>
              </div>
            </div>
            <div v-if="selectedCategoriesOnMonthlyReport[month.date] !== undefined">
              <table class="min-w-full divide-y divide-gray-300 mt-10">
                <thead class="bg-gray-100">
                  <tr>
                    <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6 text-center">
                      Nombre
                    </th>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center">
                      Pedidos
                    </th>
                    <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center">
                      Ingresos
                    </th>
                  </tr>
                </thead>
                <tbody class="divide-y divide-gray-200 bg-white">
                  <tr v-for="product in selectedCategoriesOnMonthlyReport[month.date].products" :key="product.name">
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                      {{ product.name }}
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                      {{ product.count }}
                    </td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                      {{ product.income }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </DisclosurePanel>
        </Disclosure>
      </dl>
    </section>

    <section>
      <div class="max-w-7xl mx-auto py-12 px-4 sm:px-6 lg:py-16 lg:px-8">
        <h2 class="text-center text-3xl font-extrabold text-gray-900 sm:text-4xl">
          Desglose general de productos
        </h2>
        <div v-if="productsSales !== undefined" class="mt-6 grid grid-cols-2 gap-0.5  lg:mt-8">
          <div v-for="category in productsSales.all_products" :key="category.name" class="col-span-1 flex justify-center py-8 px-8 cursor-pointer" :class="{ 'bg-gray-300': selectedCategory === category, 'bg-gray-200': selectedCategory !== category }" @click="selectedCategory = category">
            <h3>{{ category.name }}</h3>
          </div>
        </div>
      </div>

      <div v-if="selectedCategory !== undefined">
        <table class="min-w-full divide-y divide-gray-300">
          <thead class="bg-gray-100">
            <tr>
              <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6 text-center">
                Nombre
              </th>
              <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center">
                Pedidos
              </th>
              <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900 text-center">
                Ingresos
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200 bg-white">
            <tr v-for="product in selectedCategory.products" :key="product.name">
              <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">
                {{ product.name }}
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ product.count }}
              </td>
              <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                {{ product.income }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>
