<template>
  <div class="container-fluid">
    <Header />
    <slot />
  </div>
</template>


<script setup>
const user = useSupabaseUser()
</script>