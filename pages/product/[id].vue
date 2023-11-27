<script lang="ts" setup>
definePageMeta({
   middleware: "auth",
});
useHead({
   title: "Edit",
});

const route = useRoute();

const product = ref({});

await useApiFetch(`/api/product/${route.params.id}`).then((res) => {
   product.value = res.data.value.data;
});

async function handleSubmit() {
   await useApiFetch(`/api/product/update/${product.value.id}`, {
      method: "POST",
      body: {
         ...product.value,
      },
   }).then((res) => {
      if (res.status.value === "success") {
         navigateTo("/");
      }
   });
}
</script>

<template>
   <div class="w-1/3 flex flex-col gap-4">
      <h1 class="text-3xl">Edit Product</h1>
      <div class="w-full bg-white rounded-md shadow-md p-4">
         <form class="flex flex-col gap-3" @submit.prevent="handleSubmit">
            <div class="flex flex-col gap-2">
               <label for="name">Product Name</label>
               <input
                  type="text"
                  name="name"
                  id="name"
                  class="rounded-md"
                  v-model="product.name"
               />
            </div>
            <div class="flex flex-col gap-2">
               <label for="price">Price (Rp)</label>
               <input
                  type="number"
                  name="price"
                  id="price"
                  class="rounded-md"
                  v-model="product.price"
               />
            </div>
            <div class="flex flex-col gap-2">
               <label for="name">Stock</label>
               <input
                  type="number"
                  name="stock"
                  id="stock"
                  class="rounded-md"
                  v-model="product.stock"
               />
            </div>
            <div class="flex flex-col gap-2">
               <label for="name">Description</label>
               <textarea
                  v-model="product.description"
                  class="rounded-md h-28"
               ></textarea>
            </div>
            <div class="flex justify-start">
               <button
                  type="submit"
                  class="text-gray-100 bg-indigo-700 hover:bg-indigo-900 px-3 py-2 rounded-md shadow-md"
               >
                  save
               </button>
            </div>
         </form>
      </div>
   </div>
</template>

<style scoped></style>
