<template>
  <UHeader title="SATM">
    <template #title>
      <div class="flex items-center gap-2 text-primary">
        <UIcon name="i-lucide-bow-arrow" class="size-10" />
        <div class="flex flex-col">
          <span class="text-2xl">SATM</span>
          <span class="text-xs text-muted">Sebastian Aedo Task Manager</span>
        </div>
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
