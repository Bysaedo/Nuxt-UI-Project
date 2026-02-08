<template>
  <main class="flex-1 p-6">
    <div class="mb-6">
      <h2 class="text-2xl font-bold">Dashboard</h2>
      <p class="text-muted">Welcome back! {{ name }}</p>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 mb-8">
      <UCard>
        <div class="flex items-center gap-4">
          <div class="p-3 bg-primary/10 rounded-lg">
            <UIcon name="i-lucide-file-text" class="size-6 text-primary" />
          </div>
          <div>
            <p class="text-2xl font-bold">{{ totalTasks }}</p>
            <p class="text-sm text-muted">Total Tasks</p>
          </div>
        </div>
      </UCard>

      <UCard>
        <div class="flex items-center gap-4">
          <div class="p-3 bg-success/10 rounded-lg">
            <UIcon name="i-lucide-check-circle" class="size-6 text-success" />
          </div>
          <div>
            <p class="text-2xl font-bold">{{ completedTasks }}</p>
            <p class="text-sm text-muted">Completed</p>
          </div>
        </div>
      </UCard>

      <UCard>
        <div class="flex items-center gap-4">
          <div class="p-3 bg-warning/10 rounded-lg">
            <UIcon name="i-lucide-clock" class="size-6 text-warning" />
          </div>
          <div>
            <p class="text-2xl font-bold">{{ pendingTasks }}</p>
            <p class="text-sm text-muted">Pending</p>
          </div>
        </div>
      </UCard>
    </div>

    <UCard>
      <template #header>
        <div
          class="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between"
        >
          <div>
            <h3 class="font-semibold">Recent Tasks</h3>
            <p class="text-sm text-muted">Add and manage your tasks</p>
          </div>

          <div class="flex items-center gap-2 w-full sm:w-auto">
            <UInput
              v-model="newTask"
              placeholder="Add a task…"
              class="w-full sm:w-72"
              @keyup.enter="addTask"
            />
            <UButton
              icon="i-lucide-plus"
              @click="addTask"
              :disabled="!newTask.trim()"
            >
              Add
            </UButton>
          </div>
        </div>
      </template>

      <div class="space-y-4">
        <div
          v-for="task in recentTasks"
          :key="task.id"
          class="flex items-center justify-between p-3 rounded-lg hover:bg-elevated transition-colors"
        >
          <div class="flex items-center gap-3">
            <UCheckbox v-model="task.completed" />
            <div class="min-w-0">
              <p
                class="font-medium truncate"
                :class="task.completed ? 'line-through text-muted' : ''"
              >
                {{ task.title }}
              </p>
            </div>
          </div>
          <div class="flex items-center gap-2">
            <UBadge
              :label="task.priority"
              :color="priorityColor(task.priority)"
              variant="subtle"
              size="sm"
            />
            <UBadge
              :label="task.completed ? 'Complete' : 'Pending'"
              :color="task.completed ? 'success' : 'warning'"
              variant="subtle"
              size="sm"
            />
            <UDropdownMenu
              :items="[
                {
                  label: task.completed ? 'Mark pending' : 'Mark complete',
                  icon: 'i-lucide-check',
                  onSelect: () => toggle(task.id),
                },
                {
                  label: 'Delete',
                  icon: 'i-lucide-trash-2',
                  onSelect: () => askDelete(task.id),
                },
              ]"
            >
              <UButton
                icon="i-lucide-more-horizontal"
                color="neutral"
                variant="ghost"
                size="xs"
              />
            </UDropdownMenu>
            <UModal
              v-model:open="confirmDelete"
              title="Confirm Deletion"
              :ui="{ width: 'max-w-md' }"
            >
              <template #body>
                <p class="text-muted">
                  Are you sure you want to delete this item? This action cannot
                  be undone.
                </p>
              </template>
              <template #footer>
                <div class="flex justify-end gap-3">
                  <UButton
                    color="neutral"
                    variant="outline"
                    @click="confirmDelete = false"
                    >Cancel</UButton
                  >
                  <UButton color="primary" @click="handleDelete"
                    >Delete</UButton
                  >
                </div>
              </template>
            </UModal>
          </div>
        </div>
      </div>
    </UCard>
  </main>
</template>

<script setup lang="ts">
const name = "Sebastian";
const newTask = ref("");
const isOpen = ref(false);
const confirmDelete = ref(false);
const taskToDelete = ref<string | null>(null);

type Task = {
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

const totalTasks = computed(() => tasks.value.length);
const completedTasks = computed(
  () => tasks.value.filter((t) => t.completed).length,
);
const pendingTasks = computed(
  () => tasks.value.filter((t) => !t.completed).length,
);

const recentTasks = computed(() =>
  [...tasks.value].sort((a, b) => b.createdAt - a.createdAt),
);

function addTask() {
  const title = newTask.value.trim();
  if (!title) return;

  tasks.value.unshift({
    id: crypto.randomUUID(),
    title,
    completed: false,
    createdAt: Date.now(),
    priority: "moderate",
  });

  newTask.value = "";
}

function toggle(id: string) {
  const t = tasks.value.find((x) => x.id === id);
  if (t) t.completed = !t.completed;
}

function remove(id: string) {
  tasks.value = tasks.value.filter((t) => t.id !== id);
}

function priorityColor(priority: Task["priority"]) {
  if (priority === "high") return "error";
  if (priority === "moderate") return "warning";
  return "success";
}
function askDelete(id: string) {
  taskToDelete.value = id;
  confirmDelete.value = true;
}
function handleDelete() {
  if (!taskToDelete.value) return;
  remove(taskToDelete.value);
  confirmDelete.value = false;
  taskToDelete.value = null;
}
</script>
