<script lang="ts" setup>
import { TailwindPagination } from "laravel-vue-pagination";
import { refDebounced } from "@vueuse/core";
import Swal from "sweetalert2";
import "sweetalert2/src/sweetalert2.scss";

definePageMeta({
   middleware: "auth",
});
useHead({
   title: "Transactions",
});

const products = ref([]);

const keyword = ref("");
const debounced = refDebounced(keyword, 1000);

async function getResults(page = 1) {
   const { data } = await useApiFetch(`/api/products?page=${page}`);
   products.value = data.value;
}

watchEffect(async () => {
   const { data } = await useApiFetch(
      `/api/product/search?keyword=${debounced.value}`
   );

   if (debounced.value !== "") {
      products.value = data.value;
   } else {
      getResults();
   }
});

onMounted(() => {
   getResults();
});
</script>

<template>
   <div class="w-1/2 flex flex-col gap-4">
      <h1 class="text-3xl">Products</h1>
      <div class="flex gap-4">
         <NuxtLink
            to="/create-product"
            class="bg-indigo-700 p-2 rounded-md text-white hover:bg-indigo-900 shadow-md"
            >Create</NuxtLink
         >
         <input
            type="text"
            name="search"
            class="rounded-md w-full"
            placeholder="search"
            v-model="keyword"
         />
      </div>
      <div class="w-full bg-white rounded-md shadow-md">
         <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
            <table
               class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
            >
               <thead
                  class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
               >
                  <tr>
                     <th scope="col" class="px-6 py-3">Name</th>
                     <th scope="col" class="px-6 py-3">Price</th>
                     <th scope="col" class="px-6 py-3">Stock</th>
                  </tr>
               </thead>
               <tbody>
                  <tr
                     class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
                     v-for="product in products.data"
                     :key="product.id"
                  >
                     <th
                        scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
                     >
                        {{ product.name }}
                     </th>
                     <td class="px-6 py-4">Rp.{{ product.price }}</td>
                     <td class="px-6 py-4">{{ product.stock }}</td>
                     <td class="px-6 py-4 text-right flex gap-2">
                        <NuxtLink
                           :to="`/product/${product.id}`"
                           class="font-medium text-blue-600 dark:text-blue-500 hover:underline"
                           >Edit</NuxtLink
                        >
                        <button
                           @click="deleteProduct(product.id)"
                           class="font-medium text-blue-600 dark:text-blue-500 hover:underline"
                        >
                           Delete
                        </button>
                     </td>
                  </tr>
               </tbody>
            </table>
         </div>
      </div>
      <TailwindPagination
         :data="products"
         @pagination-change-page="getResults"
         class="rounded-lg overflow-hidden"
      />
   </div>
</template>

<style scoped></style>
