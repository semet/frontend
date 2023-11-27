<script lang="ts" setup>
import { Form, Field, ErrorMessage } from "vee-validate";
import { toTypedSchema } from "@vee-validate/zod";
import * as zod from "zod";

definePageMeta({
   middleware: "auth",
});

const validationSchema = toTypedSchema(
   zod.object({
      product_id: zod
         .number()
         .nonnegative("Please provide valid amount")
         .gt(0, "Value must not be 0"),
      quantity: zod
         .number()
         .nonnegative("Please provide valid amount")
         .gt(0, "Value must not be 0"),
   })
);
const product = ref({
   product_id: "",
   quantity: 0,
});

const all = ref([]);

async function getProducts() {
   const { data } = await useApiFetch(`/api/all-products`);
   all.value = data.value;
}

onMounted(() => {
   getProducts();
});

async function handleSubmit(values: any) {
   await useApiFetch(`/api/transaction/create`, {
      method: "POST",
      body: {
         ...product.value,
      },
   }).then((res) => {
      console.log(res);
   });
}
</script>

<template>
   <div class="w-1/3 flex flex-col gap-4">
      <h1 class="text-3xl">Create Transaction</h1>
      <div class="w-full bg-white rounded-md shadow-md p-4">
         <Form
            class="flex flex-col gap-3"
            @submit="handleSubmit"
            :validation-schema="validationSchema"
         >
            <div class="flex flex-col gap-2">
               <label for="product_id">Product ID</label>
               <Field
                  as="select"
                  name="product_id"
                  id="product_id"
                  v-model="product.product_id"
                  placeholder="pilih product"
               >
                  <option value="">pilih product</option>
                  <option
                     v-for="product in all.data"
                     :value="product.id"
                     :key="product.id"
                  >
                     {{ product.name }}
                  </option>
               </Field>
               <ErrorMessage class="text-red-600" name="product_id" />
            </div>
            <div class="flex flex-col gap-2">
               <label for="quantity">Quantity (Rp)</label>
               <Field
                  type="number"
                  name="quantity"
                  id="quantity"
                  class="rounded-md"
                  v-model="product.quantity"
               />
               <ErrorMessage class="text-red-600" name="quantity" />
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
