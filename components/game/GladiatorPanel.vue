<script setup lang="ts">
import {
  Coins,
  Sparkles,
  Users,
  Briefcase,
  Swords,
  Hammer,
  Skull,
  Building2,
  Info,
} from 'lucide-vue-next'
import avatarSrc from '~/assets/avatar.jpg'
import gladiatorStallSrc from '~/assets/gladiatorstall.png'

const gladiator = {
  name: 'Anhängare Juomno',
  title: 'Grad 9',
  hp: { current: 118, max: 118 },
  ep: { current: 1675, max: 1680 },
  time: { current: 128, max: 128 },
  heal: '01:42',
  form: 110,
  silver: 770,
}

const passiveOptions = [
  { value: 'knega', label: 'Knega', icon: Briefcase },
  { value: 'stridstraning', label: 'Stridsträning', icon: Swords },
  { value: 'verkstaden', label: 'Verkstaden', icon: Hammer },
  { value: 'odjur', label: 'Odjur', icon: Skull },
  { value: 'stadsbyggnader', label: 'Stadsbyggnader', icon: Building2 },
]

const selectedPassive = ref('knega')

const standbyGladiator = {
  name: 'Väktare Tarian',
  title: 'Grad 7',
  hp: { current: 96, max: 112 },
  ep: { current: 930, max: 1440 },
  time: { current: 76, max: 128 },
}
</script>

<template>
  <aside class="space-y-4 py-4">
    <div class="px-3 flex justify-end">
      <Button variant="ghost" size="icon" class="size-7 rounded-full" title="Byt gladiator">
        <Users class="size-4 text-primary" />
      </Button>
    </div>

    <div class="px-3">
      <Card class="card-interactive surface-card-highlight overflow-hidden card-gold-accent">
        <CardContent class="p-3">
          <div class="text-center">
            <h4 class="font-semibold text-base" style="font-family: var(--font-display);">{{ gladiator.name }}</h4>
            <p class="text-xs text-muted-foreground">{{ gladiator.title }}</p>
          </div>

          <div class="my-3 portrait-frame aspect-[3/4] w-full overflow-hidden rounded-lg">
            <img
              :src="avatarSrc"
              :alt="gladiator.name"
              class="size-full object-cover object-top"
            />
          </div>

          <div class="mb-3 flex items-center justify-center gap-1 rounded-md border border-primary/25 bg-primary/8 px-2 py-1.5 text-xs font-medium text-primary">
            <Sparkles class="size-3.5" />
            Aktiv gladiator
          </div>

          <div class="space-y-2 mt-3">
            <GameStatBar label="KP" :current="gladiator.hp.current" :max="gladiator.hp.max" color="hp" tooltip="Kroppspoäng" />
            <GameStatBar label="EP" :current="gladiator.ep.current" :max="gladiator.ep.max" color="ep" tooltip="Erfarenhetspoäng" />
            <GameStatBar label="TID" :current="gladiator.time.current" :max="gladiator.time.max" color="time" tooltip="Tillgängliga rundor" />

            <div class="flex items-center justify-between rounded-md bg-muted/50 px-2.5 py-1.5">
              <span class="section-kicker">Lädd</span>
              <span class="text-xs font-bold tabular-nums text-amber-600 dark:text-amber-400">{{ gladiator.heal }}</span>
            </div>

            <div class="flex items-center justify-between rounded-md bg-muted/50 px-2.5 py-1.5">
              <span class="section-kicker">Form</span>
              <span
                class="text-xs font-bold tabular-nums"
                :class="gladiator.form >= 100 ? 'text-emerald-600 dark:text-emerald-400' : 'text-destructive'"
              >
                {{ gladiator.form }} %
              </span>
            </div>
          </div>

          <Separator class="my-3" />

          <div class="space-y-2">
            <h4 class="section-kicker">Passiva funktioner</h4>
            <Select v-model="selectedPassive">
              <SelectTrigger>
                <SelectValue />
              </SelectTrigger>
              <SelectContent>
                <SelectItem v-for="opt in passiveOptions" :key="opt.value" :value="opt.value">
                  <span class="inline-flex items-center gap-2">
                    <component :is="opt.icon" class="size-3.5 text-primary" />
                    {{ opt.label }}
                  </span>
                </SelectItem>
              </SelectContent>
            </Select>

            <NuxtLink to="/passiva-funktioner" class="inline-flex items-center gap-1 text-xs text-primary hover:underline underline-offset-2">
              <Info class="size-3.5" />
              Läs mer
            </NuxtLink>
          </div>

          <Separator class="my-3" />

          <div class="flex items-center justify-between rounded-md bg-muted/50 px-2.5 py-1.5">
            <span class="flex items-center gap-1.5 section-kicker">
              <Coins class="size-3.5 text-amber-500" />
              Silvermynt
            </span>
            <span class="text-xs font-bold tabular-nums">{{ gladiator.silver }} SM</span>
          </div>
        </CardContent>
      </Card>
    </div>

    <div class="px-3">
      <Card class="card-interactive surface-card">
        <CardContent class="p-2.5">
          <div class="flex gap-2.5">
            <div class="h-16 w-12 shrink-0 overflow-hidden rounded-md border">
              <img :src="gladiatorStallSrc" :alt="standbyGladiator.name" class="size-full object-cover object-top" />
            </div>
            <div class="min-w-0 flex-1 space-y-1.5">
              <div class="flex items-baseline justify-between gap-2">
                <p class="truncate text-xs font-semibold" style="font-family: var(--font-display);">{{ standbyGladiator.name }}</p>
                <span class="text-[0.65rem] text-muted-foreground">{{ standbyGladiator.title }}</span>
              </div>

              <div class="space-y-1">
                <div class="flex items-center gap-1 text-[0.62rem]">
                  <span class="w-6 text-muted-foreground">KP</span>
                  <div class="h-1.5 flex-1 overflow-hidden rounded-full bg-muted">
                    <div class="h-full rounded-full bg-gradient-to-r from-red-500 to-red-700" :style="{ width: `${(standbyGladiator.hp.current / standbyGladiator.hp.max) * 100}%` }" />
                  </div>
                </div>
                <div class="flex items-center gap-1 text-[0.62rem]">
                  <span class="w-6 text-muted-foreground">EP</span>
                  <div class="h-1.5 flex-1 overflow-hidden rounded-full bg-muted">
                    <div class="h-full rounded-full bg-gradient-to-r from-blue-400 to-blue-600" :style="{ width: `${(standbyGladiator.ep.current / standbyGladiator.ep.max) * 100}%` }" />
                  </div>
                </div>
                <div class="flex items-center gap-1 text-[0.62rem]">
                  <span class="w-6 text-muted-foreground">TID</span>
                  <div class="h-1.5 flex-1 overflow-hidden rounded-full bg-muted">
                    <div class="h-full rounded-full bg-gradient-to-r from-emerald-400 to-emerald-600" :style="{ width: `${(standbyGladiator.time.current / standbyGladiator.time.max) * 100}%` }" />
                  </div>
                </div>
              </div>

              <div class="pt-0.5">
                <Button size="sm" class="h-7 px-2.5 text-[0.7rem]">Byt till</Button>
              </div>
            </div>
          </div>
        </CardContent>
      </Card>
    </div>
  </aside>
</template>
