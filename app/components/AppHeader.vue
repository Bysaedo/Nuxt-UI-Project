<template>
  <UHeader>
    <template #title
      ><div class="flex items-center gap-2 text-primary">
        <UIcon name="i-lucide-bow-arrow" class="size-10" />
        <div class="flex flex-col">
          <span class="text-2xl">SATM</span>
          <span class="text-xs text-muted">Sebastian Aedo Task Manager</span>
        </div>
      </div>
    </template>
    <template #right>
      <UButton
        icon="i-lucide-search"
        variant="ghost"
        color="neutral"
        @click="open = true"
      />
      <UColorModeButton />
      <UAvatar text="SB" size="md" class="cursor-pointer" />
    </template>
    <ClientOnly>
      <UDashboardSearch
        v-model:open="open"
        v-model:search-term="searchTerm"
        :groups="groups"
        placeholder="Search tasks"
      >
      </UDashboardSearch>
    </ClientOnly>
  </UHeader>
</template>

<script setup lang="ts">
import { inject, ref, computed } from "vue";
import type {
  CommandPaletteGroup,
  CommandPaletteItem,
  NavigationMenuItem,
} from "@nuxt/ui";
const open = ref(false);
const searchTerm = ref("");
const tasks = inject<any>("tasks");
const toggle = inject<(id: string) => void>("toggle");
const requestEditTask = inject<(id: string) => void>("requestEditTask");

type Task = {
  id: string;
  title: string;
  completed: boolean;
  createdAt: number;
  priority: "low" | "moderate" | "high";
};
if (!tasks || !toggle || !requestEditTask) {
  throw new Error("Tasks not provided from layout");
}

const groups = computed<CommandPaletteGroup<CommandPaletteItem>[]>(() => [
  {
    id: "tasks",
    label: "Tasks",
    items: (tasks.value as Task[]).map((t) => ({
      id: t.id,
      label: t.title,
      icon: t.completed ? "i-lucide-check-circle" : "i-lucide-circle",
      description: t.priority,
      onSelect: () => {
        requestEditTask(t.id);
        open.value = false;
      },
    })),
  },
]);
</script>
