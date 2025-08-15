<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav class="flex justify-between gap-4 mb-8 max-w-7xl mx-auto h-14 items-center">
        <p class="bg-blue-900 text-white h-full px-4 m-0 flex items-center text-2xl">
          Wonder.
          <span class="font-bold pl-1">Legal</span>
        </p>
        <div class="flex gap-4 items-center font-semibold">
          <RouterLink to="/" custom v-slot="{ href, navigate, isExactActive }">
            <a
              :href="href"
              @click="navigate"
              class="transition"
              :class="
                isExactActive
                  ? 'text-[#08ECAF] text-xs md:text-sm'
                  : 'text-blue-900 hover:text-[#08ECAF] text-xs md:text-sm'
              "
            >
              INICIO
            </a>
          </RouterLink>
          <RouterLink to="/creation-model" custom v-slot="{ href, navigate, isActive }">
            <a
              :href="href"
              @click="navigate"
              class="transition"
              :class="
                isActive
                  ? 'text-[#08ECAF] text-xs md:text-sm'
                  : 'text-blue-900 hover:text-[#08ECAF] text-xs md:text-sm'
              "
            >
              DOCUMENTOS
            </a>
          </RouterLink>
          <RouterLink to="/multi-form" custom v-slot="{ href, navigate, isActive }">
            <a
              :href="href"
              @click="navigate"
              class="transition"
              :class="
                isActive
                  ? 'text-[#08ECAF] text-xs md:text-sm'
                  : 'text-blue-900 hover:text-[#08ECAF] text-xs md:text-sm'
              "
            >
              FORMULARIO
            </a>
          </RouterLink>
        </div>
        <div>
          <input type="text" placeholder="Buscar..." class="border p-1 rounded hidden md:block" />
        </div>
      </nav>
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-3xl md:text-4xl font-bold text-gray-900 mb-2">
          Professional Service Request
        </h1>
        <p class="text-gray-600">Complete this form to get started with your project</p>
      </div>
    </div>
  </header>

  <RouterView />
</template>

<style scoped></style>
