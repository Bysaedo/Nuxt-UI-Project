<template>
  <UHeader title="SATM">
    <template #title>
      <div class="flex items-center gap-2">
        <UIcon name="i-lucide-flame" class="size-5" />
        <span>SATM</span>
      </div>
    </template>

    <UNavigationMenu :items="items" />

    <template #right>
      <UColorModeButton />
      <UAvatar text="SB" size="md" />
    </template>
  </UHeader>
</template>

<script setup lang="ts">
import type { NavigationMenuItem } from "@nuxt/ui";

const items = ref<NavigationMenuItem[]>([
  {
    label: "Home",
    icon: "i-lucide-house",
    to: "/",
  },
  {
    label: "Search tasks",
    icon: "i-lucide-search",
    to: "/searchPage",
  },
]);
</script>
