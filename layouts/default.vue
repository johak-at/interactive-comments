<template>
  <v-main class="main">
    <!-- <UiBreadcrumbs /> NOT WORKING AS INTENDED - DON'T USE -->
    <slot></slot>
  </v-main>
</template>

<style scoped>
.main {
  background-color: #f5f5f5;
}
</style>
