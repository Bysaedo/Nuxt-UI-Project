<template>
  <UFooter>
    <p>Copyright © {{ new Date().getFullYear() }}</p>
    <UNavigationMenu
      :items="items"
      variant="link"
    />
  </UFooter>
</template>

<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

const items: NavigationMenuItem[] = [
  {
    label: 'GitHub',
    to: 'https://github.com/Bysaedo',
    target: '_blank',
    icon: 'i-simple-icons-github'
  },
  {
    label: 'LinkedIn',
    to: 'https://linkedin.com/in/sebastian-aedo-vidal',
    target: '_blank',
    icon: 'i-simple-icons-linkedin'
  }
]
</script>
