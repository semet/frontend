<script lang="ts" setup>
import { useAuthStore } from "~/stores/useAuthStore";
definePageMeta({
   layout: "auth",
   middleware: "guest",
});
useHead({
   title: "Login",
});

const form = ref({
   email: "",
   password: "",
});

const auth = useAuthStore();

async function handleLogin() {
   const { error, data } = await auth.login(form.value);

   console.log(data);
}
</script>

<template>
   <form class="flex flex-col gap-3" @submit.prevent="handleLogin">
      <div class="flex flex-col gap-2">
         <label for="email">Email</label>
         <input
            type="email"
            name="email"
            id="email"
            class="rounded-md"
            v-model="form.email"
         />
      </div>
      <div class="flex flex-col gap-2">
         <label for="password">Password</label>
         <input
            type="password"
            name="password"
            id="password"
            class="rounded-md"
            v-model="form.password"
         />
      </div>

      <div class="flex justify-center">
         <button
            class="text-gray-100 bg-indigo-700 hover:bg-indigo-900 px-3 py-2 rounded-md shadow-md mx-auto"
         >
            Login
         </button>
      </div>
   </form>
</template>

<style scoped></style>
