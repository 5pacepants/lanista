<script setup lang="ts">
import { HelpCircle } from 'lucide-vue-next'

interface Props {
  label: string
  current: number
  max: number
  color?: 'hp' | 'ep' | 'time' | 'heal' | 'form'
  tooltip?: string
}

const props = withDefaults(defineProps<Props>(), {
  color: 'hp',
})

const percentage = computed(() => Math.min(100, (props.current / props.max) * 100))
const isLow = computed(() => props.color === 'hp' && percentage.value < 30)

const barClasses: Record<string, string> = {
  hp: 'from-red-500 to-red-700 dark:from-red-500 dark:to-red-800',
  ep: 'from-blue-400 to-blue-600 dark:from-blue-400 dark:to-blue-700',
  time: 'from-emerald-400 to-emerald-600 dark:from-emerald-400 dark:to-emerald-700',
  heal: 'from-amber-400 to-amber-600 dark:from-amber-400 dark:to-amber-700',
  form: 'from-emerald-400 to-emerald-600 dark:from-emerald-400 dark:to-emerald-700',
}
</script>

<template>
  <div class="space-y-0.5">
    <div class="flex items-center gap-1 text-xs">
      <span class="font-semibold tracking-wide text-muted-foreground uppercase" style="font-size: 0.65rem;">
        {{ label }}
      </span>
      <Tooltip v-if="tooltip">
        <TooltipTrigger as-child>
          <HelpCircle class="size-3 text-muted-foreground/60 cursor-help" />
        </TooltipTrigger>
        <TooltipContent>{{ tooltip }}</TooltipContent>
      </Tooltip>
    </div>
    <div
      class="relative h-5 w-full overflow-hidden rounded-full bg-muted shadow-inner"
      :class="{ 'pulse-danger': isLow }"
    >
      <!-- Gradient fill -->
      <div
        class="stat-bar-fill h-full rounded-full bg-gradient-to-r shadow-sm transition-all duration-500"
        :class="barClasses[color]"
        :style="{ width: `${percentage}%` }"
      />
      <!-- Subtle shine overlay -->
      <div class="absolute inset-0 rounded-full bg-gradient-to-b from-white/20 to-transparent pointer-events-none" />
      <!-- Text overlay on bar -->
      <div class="absolute inset-0 flex items-center justify-center text-[0.65rem] font-bold tabular-nums">
        <span class="drop-shadow-sm" :class="percentage > 45 ? 'text-white' : 'text-foreground'">
          {{ current }} / {{ max }}
        </span>
      </div>
    </div>
  </div>
</template>
