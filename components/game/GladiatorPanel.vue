<script setup lang="ts">
import { Info, BookOpen, Gem, History, BarChart3, Shirt, Briefcase, Award, Activity, Coins, Shield } from 'lucide-vue-next'

const gladiator = {
  name: 'Anhängare Juomno',
  title: 'Grad 9',
  image: '/gladiator-placeholder.png',
  capacity: { current: 328, max: 328 },
  hp: { current: 118, max: 118 },
  ep: { current: 1675, max: 1680 },
  time: { current: 128, max: 128 },
  heal: '01:42',
  form: 110,
  silver: 770,
}

const sideLinks = [
  { label: 'Info', icon: Info, href: '/avatar/info' },
  { label: 'Biografi', icon: BookOpen, href: '/avatar/biography' },
  { label: 'Egenskaper', icon: Gem, href: '/avatar/properties' },
  { label: 'Historia', icon: History, href: '/avatar/history' },
  { label: 'Statistik', icon: BarChart3, href: '/avatar/statistics' },
  { label: 'Utrustning', icon: Shirt, href: '/avatar/equipment' },
  { label: 'Yrken', icon: Briefcase, href: '/avatar/professions' },
  { label: 'Bedrifter', icon: Award, href: '/avatar/achievements' },
  { label: 'Aktivitet', icon: Activity, href: '/avatar/activity' },
]

const route = useRoute()
</script>

<template>
  <aside class="space-y-4 py-4">
    <!-- Gladiator Stall Header -->
    <div class="px-3">
      <div class="flex items-center justify-between">
        <h3 class="text-xs font-semibold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display);">
          Gladiatorstall
        </h3>
        <span class="text-xs text-muted-foreground tabular-nums">
          {{ gladiator.capacity.current }}/{{ gladiator.capacity.max }}
        </span>
      </div>
    </div>

    <!-- Gladiator Card -->
    <div class="px-3">
      <Card class="card-interactive overflow-hidden card-gold-accent">
        <CardContent class="p-3">
          <div class="text-center">
            <h4 class="font-semibold text-sm" style="font-family: var(--font-display);">{{ gladiator.name }}</h4>
            <p class="text-xs text-muted-foreground">{{ gladiator.title }}</p>
          </div>

          <!-- Portrait with decorative frame -->
          <div class="my-3 portrait-frame aspect-[3/4] w-full overflow-hidden rounded-lg bg-gradient-to-b from-muted to-muted/60 flex items-center justify-center">
            <Shield class="size-16 text-muted-foreground/20" />
          </div>

          <!-- Stat Bars -->
          <div class="space-y-2 mt-4">
            <GameStatBar label="KP" :current="gladiator.hp.current" :max="gladiator.hp.max" color="hp" tooltip="Kroppspoäng" />
            <GameStatBar label="EP" :current="gladiator.ep.current" :max="gladiator.ep.max" color="ep" tooltip="Erfarenhetspoäng" />
            <GameStatBar label="TID" :current="gladiator.time.current" :max="gladiator.time.max" color="time" tooltip="Tillgängliga rundor" />

            <!-- Heal timer -->
            <div class="flex items-center justify-between rounded-md bg-muted/50 px-2.5 py-1.5">
              <span class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Lädd</span>
              <span class="text-xs font-bold tabular-nums text-amber-600 dark:text-amber-400">{{ gladiator.heal }}</span>
            </div>

            <!-- Form -->
            <div class="flex items-center justify-between rounded-md bg-muted/50 px-2.5 py-1.5">
              <span class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Form</span>
              <span
                class="text-xs font-bold tabular-nums"
                :class="gladiator.form >= 100 ? 'text-emerald-600 dark:text-emerald-400' : 'text-destructive'"
              >
                {{ gladiator.form }} %
              </span>
            </div>
          </div>

          <Separator class="my-3" />

          <!-- Silver -->
          <div class="flex items-center justify-between rounded-md bg-muted/50 px-2.5 py-1.5">
            <span class="flex items-center gap-1.5 text-xs font-semibold uppercase text-muted-foreground" style="font-size: 0.65rem;">
              <Coins class="size-3.5 text-amber-500" />
              Silvermynt
            </span>
            <span class="text-xs font-bold tabular-nums">{{ gladiator.silver }} SM</span>
          </div>
        </CardContent>
      </Card>
    </div>

    <!-- Quick Nav with active state -->
    <div class="px-3">
      <h4 class="mb-1 px-1 text-xs font-semibold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
        {{ gladiator.name }}
      </h4>
      <div class="space-y-0.5">
        <NuxtLink
          v-for="link in sideLinks"
          :key="link.label"
          :to="link.href"
          class="nav-item flex items-center gap-2 rounded-md px-2 py-1.5 text-sm transition-colors"
          :class="route.path === link.href
            ? 'bg-accent/60 text-accent-foreground font-medium border-l-2 border-primary'
            : 'hover:bg-accent/30 text-muted-foreground hover:text-foreground'"
        >
          <component :is="link.icon" class="size-4 shrink-0" :class="route.path === link.href ? 'text-primary' : 'text-muted-foreground'" />
          <span>{{ link.label }}</span>
        </NuxtLink>
      </div>
    </div>
  </aside>
</template>
