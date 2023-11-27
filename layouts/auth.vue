<script lang="ts" setup></script>

<template>
   <div
      class="absolute top-[20%] left-[50%] transform -translate-x-[50%] -translate-y-[50%] w-1/4 flex flex-col gap-4"
   >
      <h1 class="text-center text-5xl font-semibold">Login</h1>
      <div class="bg-white p-6 rounded-md shadow-md">
         <slot />
      </div>
   </div>
</template>

<style>
html {
   background-color: #f1f5f9;
}
</style>
