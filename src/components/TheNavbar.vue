<script setup>
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import { MenuIcon, XIcon } from '@heroicons/vue/outline/index.js'

defineProps({
  navigation: { type: Array, required: true },
})

const emit = defineEmits(['changedSelectedNavbarItem'])
</script>

<template>
  <div>
    <Disclosure v-slot="{ open }" as="nav" class="bg-gray-800">
      <div class="max-w-7xl mx-auto px-2 md:px-6 lg:px-8 md:flex md:flex-row md:justify-center">
        <div class="relative flex items-center justify-between h-16">
          <div class="absolute inset-y-0 left-0 flex items-center md:hidden">
            <!-- Mobile menu button -->
            <DisclosureButton class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white">
              <span class="sr-only">Open main menu</span>
              <MenuIcon v-if="!open" class="block h-6 w-6" aria-hidden="true" />
              <XIcon v-else class="block h-6 w-6" aria-hidden="true" />
            </DisclosureButton>
          </div>
          <div class="flex-1 flex items-center justify-center md:items-stretch md:justify-start">
            <div class="flex-shrink-0 flex items-center">
              <img class="block lg:hidden h-8 w-auto" src="https://tailwindui.com/img/logos/workflow-mark-indigo-500.svg" alt="Workflow">
              <img class="hidden lg:block h-8 w-auto" src="https://tailwindui.com/img/logos/workflow-logo-indigo-500-mark-white-text.svg" alt="Workflow">
            </div>
            <div class="hidden md:block md:ml-6">
              <div class="flex space-x-4">
                <div v-for="item in navigation" :key="item.name">
                  <router-link :to="item.to">
                    <button class="px-3 py-2 rounded-md text-sm font-medium" :class="[item.current ? 'bg-gray-900 text-white' : 'text-gray-300 hover:bg-gray-700 hover:text-white']" @click="$emit('changedSelectedNavbarItem', $event, item.name)">
                      {{ item.name }}
                    </button>
                  </router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <DisclosurePanel class="md:hidden">
        <div class="px-2 pt-2 pb-3 space-y-1">
          <div v-for="item in navigation" :key="item.name">
            <router-link :to="item.to">
              <DisclosureButton class="block px-3 py-2 rounded-md text-base font-medium" :class="[item.current ? 'bg-gray-900 text-white' : 'text-gray-300 hover:bg-gray-700 hover:text-white']" :aria-current="item.current ? 'page' : undefined" @click="changeNavigationItemHandler(item.name)">
                {{ item.name }}
              </DisclosureButton>
            </router-link>
          </div>
        </div>
      </DisclosurePanel>
    </Disclosure>
  </div>
</template>
