<script setup lang="ts">
import {
  HelpCircle,
  Info,
  BookOpen,
  Gem,
  History,
  BarChart3,
  Shirt,
  Briefcase,
  Star,
  Activity,
  Swords,
  Skull,
  Crown,
  Shield,
  Sparkles,
} from 'lucide-vue-next'

const gladiator = {
  profile: 'Fasaden',
  name: 'Anhängare Juomno',
  online: true,
  title: 'Lärling',
  ranking: '#1233',
  rankingPoints: '0 (6vs6)',
  maxRP: '0 (6vs6)',
  reputation: 0,
  reputationType: 'Negativ',
  race: 'Salamanth',
  age: 'Ung (1 dag)',
  gender: 'Kvinna',
  clan: 'Antuins Anhängare',
  grade: 9,
  experience: '7 968 EP',
  physical: 'Utmärkt',
  agility: 'Bra',
  intellect: 'Bedrövlig',
}

const tabs = [
  { value: 'info', label: 'Info', icon: Info },
  { value: 'biography', label: 'Biografi', icon: BookOpen },
  { value: 'properties', label: 'Egenskaper', icon: Gem },
  { value: 'history', label: 'Historia', icon: History },
  { value: 'statistics', label: 'Statistik', icon: BarChart3 },
  { value: 'equipment', label: 'Utrustning', icon: Shirt },
  { value: 'professions', label: 'Yrken', icon: Briefcase },
  { value: 'reputation', label: 'Rykte', icon: Star },
  { value: 'achievements', label: 'Bedrifter', icon: Activity },
]

const tacticsOptions = ['Normal', 'Aggressiv', 'Defensiv', 'Försiktig']
const giveUpOptions = ['10% av din KP', '20% av din KP', '30% av din KP (~35 KP)', '50% av din KP']

const selectedTactic = ref('Normal')
const selectedGiveUp = ref('30% av din KP (~35 KP)')
const selectedTeamStart = ref('90% av din KP (~106 KP)')
const restoreAfterTournament = ref(true)

const selectedBeastTactic = ref('Normal')
const selectedBeastGiveUp = ref('20% av din KP (~23 KP)')

const battleSettingsSaved = ref(false)
const beastSettingsSaved = ref(false)

function showSaved(flag: { value: boolean }) {
  flag.value = true
  setTimeout(() => {
    flag.value = false
  }, 1400)
}

watch([selectedTactic, selectedGiveUp, selectedTeamStart, restoreAfterTournament], () => {
  showSaved(battleSettingsSaved)
})

watch([selectedBeastTactic, selectedBeastGiveUp], () => {
  showSaved(beastSettingsSaved)
})
</script>

<template>
  <div class="space-y-5">
    <section class="hero-shell rounded-xl p-4 lg:p-5">
      <div class="space-y-2.5">
        <h1 class="text-3xl font-bold tracking-tight lg:text-4xl" style="font-family: var(--font-display);">
          Min Gladiator
        </h1>
        <div class="flex flex-wrap gap-2">
          <div class="hero-stat-pill">
            <Crown class="size-3.5 text-primary" />
            <span>Grad {{ gladiator.grade }}</span>
          </div>
          <div class="hero-stat-pill">
            <Shield class="size-3.5 text-primary" />
            <span>Ranking {{ gladiator.ranking }}</span>
          </div>
          <div class="hero-stat-pill">
            <Sparkles class="size-3.5 text-primary" />
            <span>{{ gladiator.experience }}</span>
          </div>
        </div>
      </div>
    </section>

    <Tabs default-value="info">
      <div class="tabs-scroll-container overflow-x-auto pb-1 -mx-1 px-1">
        <TabsList class="inline-flex gap-1 w-max bg-transparent p-0">
          <TabsTrigger
            v-for="tab in tabs"
            :key="tab.value"
            :value="tab.value"
            class="tab-trigger text-xs sm:text-sm gap-1.5 shrink-0 border border-border/60 bg-background"
          >
            <component :is="tab.icon" class="size-3.5 hidden sm:block" />
            {{ tab.label }}
          </TabsTrigger>
        </TabsList>
      </div>

      <TabsContent value="info" class="mt-4 space-y-5">
        <Card class="card-interactive surface-card-highlight card-gold-accent">
          <CardContent class="py-5">
            <div class="grid gap-5 md:grid-cols-2">
              <div class="space-y-3">
                <div class="space-y-2">
                  <h3 class="section-kicker">Identitet</h3>
                  <div class="grid grid-cols-[130px_1fr] gap-y-2.5 gap-x-3 text-[0.95rem]">
                    <span class="text-muted-foreground font-medium">Profil</span>
                    <span>{{ gladiator.profile }}</span>

                    <span class="text-muted-foreground font-medium">Namn</span>
                    <span class="flex items-center gap-1.5">
                      <strong class="text-base">{{ gladiator.name }}</strong>
                      <span v-if="gladiator.online" class="flex items-center gap-1">
                        <span class="relative flex size-2">
                          <span class="absolute inline-flex size-full animate-ping rounded-full bg-green-400 opacity-75" />
                          <span class="relative inline-flex size-2 rounded-full bg-green-500" />
                        </span>
                        <span class="text-xs text-green-600 dark:text-green-400">Online</span>
                      </span>
                    </span>

                    <span class="text-muted-foreground font-medium">Titel</span>
                    <span>{{ gladiator.title }}</span>

                    <span class="text-muted-foreground font-medium">Ras</span>
                    <span class="font-medium">{{ gladiator.race }}</span>

                    <span class="text-muted-foreground font-medium">Ålder</span>
                    <span>{{ gladiator.age }}</span>

                    <span class="text-muted-foreground font-medium">Kön</span>
                    <span>{{ gladiator.gender }}</span>

                    <span class="text-muted-foreground font-medium">Ranking</span>
                    <span class="tabular-nums font-semibold">{{ gladiator.ranking }}</span>

                    <span class="text-muted-foreground font-medium">Rankingpoäng</span>
                    <span class="flex items-center gap-1 tabular-nums">
                      {{ gladiator.rankingPoints }}
                      <Tooltip>
                        <TooltipTrigger as-child>
                          <HelpCircle class="size-3 text-muted-foreground/50 cursor-help" />
                        </TooltipTrigger>
                        <TooltipContent>Rankingpoäng baserat på 6vs6 strider</TooltipContent>
                      </Tooltip>
                    </span>

                    <span class="text-muted-foreground font-medium">Max RP</span>
                    <span class="tabular-nums">{{ gladiator.maxRP }}</span>

                    <span class="text-muted-foreground font-medium">Rykte</span>
                    <span class="tabular-nums">{{ gladiator.reputation }}</span>

                    <span class="text-muted-foreground font-medium">Ryktestyp</span>
                    <span class="text-destructive font-medium">{{ gladiator.reputationType }}</span>

                    <span class="text-muted-foreground font-medium">Klan</span>
                    <NuxtLink to="#" class="text-primary font-medium hover:underline underline-offset-2">{{ gladiator.clan }}</NuxtLink>
                  </div>
                </div>

                <Separator />

                <div class="space-y-2">
                  <h3 class="section-kicker">Grundegenskaper</h3>
                  <div class="flex flex-wrap gap-2">
                    <div class="flex items-center gap-2 rounded-lg border bg-card px-3 py-2">
                      <span class="text-xs text-muted-foreground">Fysik</span>
                      <span class="text-sm font-bold text-emerald-600 dark:text-emerald-400">{{ gladiator.physical }}</span>
                    </div>
                    <div class="flex items-center gap-2 rounded-lg border bg-card px-3 py-2">
                      <span class="text-xs text-muted-foreground">Smidighet</span>
                      <span class="text-sm font-bold text-blue-600 dark:text-blue-400">{{ gladiator.agility }}</span>
                    </div>
                    <div class="flex items-center gap-2 rounded-lg border bg-card px-3 py-2">
                      <span class="text-xs text-muted-foreground">Intellekt</span>
                      <span class="text-sm font-bold text-red-600 dark:text-red-400">{{ gladiator.intellect }}</span>
                    </div>
                  </div>
                </div>
              </div>

              <div class="hidden md:block space-y-4">
                <GameMeritSection />
              </div>
            </div>
          </CardContent>
        </Card>

        <Card class="md:hidden card-interactive surface-card">
          <CardContent class="pt-4 pb-4 space-y-4">
            <GameMeritSection />
          </CardContent>
        </Card>

        <Card class="card-interactive surface-card">
          <CardHeader class="pb-3">
            <CardTitle class="text-base sm:text-lg" style="font-family: var(--font-display);">
              Standardtaktik
            </CardTitle>
          </CardHeader>
          <CardContent class="pt-3 pb-2">
            <div class="grid gap-4 lg:grid-cols-2">
              <section class="space-y-3">
                <h4 class="flex items-center gap-2 text-sm font-semibold" style="font-family: var(--font-display);">
                  <Swords class="size-4 text-primary" />
                  Strider
                </h4>
                <div class="grid gap-3 sm:grid-cols-2">
                  <div class="space-y-2">
                    <label class="section-kicker">Taktik</label>
                    <Select v-model="selectedTactic">
                      <SelectTrigger>
                        <SelectValue />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem v-for="opt in tacticsOptions" :key="opt" :value="opt">{{ opt }}</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>

                  <div class="space-y-2">
                    <label class="section-kicker">Ge upp vid</label>
                    <Select v-model="selectedGiveUp">
                      <SelectTrigger>
                        <SelectValue />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem v-for="opt in giveUpOptions" :key="opt" :value="opt">{{ opt }}</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>
                </div>

                <div class="space-y-2">
                  <label class="section-kicker">Tillåt lagspel att starta vid</label>
                  <Select v-model="selectedTeamStart">
                    <SelectTrigger>
                      <SelectValue />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="90% av din KP (~106 KP)">90% av din KP (~106 KP)</SelectItem>
                      <SelectItem value="80% av din KP (~94 KP)">80% av din KP (~94 KP)</SelectItem>
                      <SelectItem value="70% av din KP (~82 KP)">70% av din KP (~82 KP)</SelectItem>
                    </SelectContent>
                  </Select>
                </div>

                <div class="flex items-center gap-2 pt-1">
                  <Checkbox id="restore-tournament" v-model:checked="restoreAfterTournament" />
                  <label for="restore-tournament" class="text-sm cursor-pointer">Återställ efter turneringsstrid</label>
                </div>
                <p v-if="battleSettingsSaved" class="saved-indicator mt-1">Inställning sparad</p>
              </section>

              <section class="space-y-3 lg:border-l lg:border-border/70 lg:pl-5">
                <h4 class="flex items-center gap-2 text-sm font-semibold" style="font-family: var(--font-display);">
                  <Skull class="size-4 text-primary" />
                  Odjursstrider
                </h4>
                <div class="grid gap-3 sm:grid-cols-2">
                  <div class="space-y-2">
                    <label class="section-kicker">Taktik mot odjur</label>
                    <Select v-model="selectedBeastTactic">
                      <SelectTrigger>
                        <SelectValue />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem v-for="opt in tacticsOptions" :key="opt" :value="opt">{{ opt }}</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>

                  <div class="space-y-2">
                    <label class="section-kicker">Ge upp vid (odjur)</label>
                    <Select v-model="selectedBeastGiveUp">
                      <SelectTrigger>
                        <SelectValue />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem v-for="opt in giveUpOptions" :key="opt" :value="opt">{{ opt }}</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>
                </div>
                <p v-if="beastSettingsSaved" class="saved-indicator mt-1">Inställning sparad</p>
              </section>
            </div>
          </CardContent>
        </Card>
      </TabsContent>

      <TabsContent v-for="tab in tabs.filter(t => t.value !== 'info')" :key="tab.value" :value="tab.value" class="mt-6">
        <Card class="card-interactive surface-card">
          <CardContent class="flex flex-col items-center justify-center gap-3 py-16">
            <component :is="tab.icon" class="size-10 text-muted-foreground/30" />
            <p class="text-muted-foreground" style="font-family: var(--font-display);">{{ tab.label }}</p>
            <p class="text-xs text-muted-foreground/60">Kommer snart</p>
          </CardContent>
        </Card>
      </TabsContent>
    </Tabs>
  </div>
</template>
