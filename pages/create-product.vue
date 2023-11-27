<script lang="ts" setup>
import { Form, Field, ErrorMessage } from "vee-validate";
import { toTypedSchema } from "@vee-validate/zod";
import * as zod from "zod";
const validationSchema = toTypedSchema(
   zod.object({
      name: zod.string().nonempty("This is required"),
      price: zod.string().nonempty("This is required"),
      stock: zod.string().nonempty("This is required"),
   })
);
const product = ref({
   name: "",
   price: "",
   stock: "",
   description: "",
});

async function handleSubmit(values: any) {
   await useApiFetch("/api/product/store", {
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
      <h1 class="text-3xl">Create Product</h1>
      <div class="w-full bg-white rounded-md shadow-md p-4">
         <Form
            class="flex flex-col gap-3"
            @submit="handleSubmit"
            :validation-schema="validationSchema"
         >
            <div class="flex flex-col gap-2">
               <label for="name">Product Name</label>
               <Field
                  type="text"
                  name="name"
                  id="name"
                  class="rounded-md"
                  v-model="product.name"
               />
               <ErrorMessage class="text-red-600" name="name" />
            </div>
            <div class="flex flex-col gap-2">
               <label for="price">Price (Rp)</label>
               <Field
                  type="text"
                  name="price"
                  id="price"
                  class="rounded-md"
                  v-model="product.price"
               />
               <ErrorMessage class="text-red-600" name="price" />
            </div>
            <div class="flex flex-col gap-2">
               <label for="name">Stock</label>
               <Field
                  type="text"
                  name="stock"
                  id="stock"
                  class="rounded-md"
                  v-model="product.stock"
               />
               <ErrorMessage class="text-red-600" name="stock" />
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
         </Form>
      </div>
   </div>
</template>

<style scoped></style>
