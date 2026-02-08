<template>
  <div class="min-h-screen flex flex-col">
    <AppHeader />
    <main class="flex-1">
      <slot />
    </main>
    <AppFooter />
  </div>
</template>
<script setup lang="ts">
export type Task = {
  id: string;
  title: string;
  completed: boolean;
  createdAt: number;
  priority: "low" | "moderate" | "high";
};

const tasks = ref<Task[]>([
  {
    id: "1",
    title: "Laundry",
    completed: true,
    createdAt: Date.now(),
    priority: "high",
  },
  {
    id: "2",
    title: "Groceries",
    completed: false,
    createdAt: Date.now(),
    priority: "moderate",
  },
  {
    id: "3",
    title: "Dishes",
    completed: false,
    createdAt: Date.now(),
    priority: "low",
  },
  {
    id: "4",
    title: "Homework",
    completed: true,
    createdAt: Date.now(),
    priority: "high",
  },
  {
    id: "5",
    title: "Clean floors",
    completed: false,
    createdAt: Date.now(),
    priority: "low",
  },
  {
    id: "6",
    title: "Wash car",
    completed: true,
    createdAt: Date.now(),
    priority: "low",
  },
  {
    id: "7",
    title: "Gym",
    completed: true,
    createdAt: Date.now(),
    priority: "moderate",
  },
  {
    id: "8",
    title: "Shower",
    completed: false,
    createdAt: Date.now(),
    priority: "moderate",
  },
  {
    id: "9",
    title: "Dinner",
    completed: false,
    createdAt: Date.now(),
    priority: "low",
  },
  {
    id: "10",
    title: "Nap",
    completed: true,
    createdAt: Date.now(),
    priority: "high",
  },
]);

function toggle(id: string) {
  const t = tasks.value.find((x) => x.id === id);
  if (t) t.completed = !t.completed;
}

function remove(id: string) {
  tasks.value = tasks.value.filter((t) => t.id !== id);
}

const requestEditTaskId = ref<string | null>(null);
function requestEditTask(id: string) {
  requestEditTaskId.value = id;
}

provide("tasks", tasks);
provide("toggle", toggle);
provide("remove", remove);
provide("requestEditTaskId", requestEditTaskId);
provide("requestEditTask", requestEditTask);
</script>
