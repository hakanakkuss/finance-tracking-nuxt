<script setup>
// Link to Google Fonts
import AppHeader from "~/components/app-header.vue";

useHead({
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com'
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Inter&display=swap',
      crossorigin: ''
    }
  ]
})
</script>


<template>
  <div class="container mx-auto max-w-4xl mt-10">
    <AppHeader/>
    <main class="my-10">
      <slot />
    </main>
  </div>
</template>



<style>
/* Set the font family for the entire site */
body {
  font-family: 'Inter';
}
/* Set the background color for the entire site */
body {
  @apply dark:bg-gray-900 bg-white
}
</style>
