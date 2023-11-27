<script lang="ts" setup></script>

<template>
   <div>
      <header class="bg-indigo-400 px-4 mb-10">
         <ul class="flex">
            <li
               class="hover:bg-white p-4 text-white hover:text-gray-700 font-semibold"
            >
               <NuxtLink to="/">Home</NuxtLink>
            </li>
            <li
               class="hover:bg-white p-4 text-white hover:text-gray-700 font-semibold"
            >
               <NuxtLink to="/products">Products</NuxtLink>
            </li>
            <li
               class="hover:bg-white p-4 text-white hover:text-gray-700 font-semibold"
            >
               <NuxtLink to="/transactions">Transactions</NuxtLink>
            </li>
         </ul>
      </header>
      <div class="p-4">
         <slot />
      </div>
   </div>
</template>

<style scoped></style>
