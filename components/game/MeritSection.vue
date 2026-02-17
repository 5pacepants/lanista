<script setup lang="ts">
import { ChevronDown, Trophy, Sword, TreePine, Mountain, Crown } from 'lucide-vue-next'

interface Merit {
  name: string
  count: number
  icon: any
  items?: string[]
}

const merits: Merit[] = [
  { name: 'Veteran (grad 45)', count: 13, icon: Sword },
  { name: 'Mästarnas mästare', count: 1, icon: Crown },
  { name: 'Besegrade "Tusenåriga skogen"', count: 1, icon: TreePine },
  { name: 'Legend', count: 3, icon: Trophy },
  { name: 'Besegrade "Monsterjakt i Bergspasset"', count: 1, icon: Mountain },
]

const openMerits = ref<string[]>([])

function toggleMerit(name: string) {
  const idx = openMerits.value.indexOf(name)
  if (idx >= 0) {
    openMerits.value.splice(idx, 1)
  } else {
    openMerits.value.push(name)
  }
}
</script>

<template>
  <div class="space-y-3">
    <h3 class="text-sm font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.7rem;">
      Meriter
    </h3>
    <div class="space-y-1">
      <div v-for="merit in merits" :key="merit.name">
        <button
          class="group flex w-full items-center gap-2 rounded-lg px-3 py-2.5 text-sm hover:bg-accent/40 transition-all"
          @click="toggleMerit(merit.name)"
        >
          <component :is="merit.icon" class="size-4 text-primary/70 shrink-0" />
          <span class="flex-1 text-left text-sm">{{ merit.name }}</span>
          <Badge variant="secondary" class="h-5 min-w-5 justify-center rounded-full px-1.5 text-[0.6rem] font-bold tabular-nums">
            {{ merit.count }}
          </Badge>
          <ChevronDown
            class="size-3.5 text-muted-foreground transition-transform duration-200 shrink-0"
            :class="{ 'rotate-180': openMerits.includes(merit.name) }"
          />
        </button>
        <div
          v-if="openMerits.includes(merit.name) && merit.items"
          class="ml-6 border-l-2 border-primary/20 pl-3 py-1 text-sm text-muted-foreground"
        >
          <p v-for="item in merit.items" :key="item" class="py-0.5">{{ item }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
