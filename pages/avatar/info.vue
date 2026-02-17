<script setup lang="ts">
import {
  HelpCircle, Info, BookOpen, Gem, History, BarChart3,
  Shirt, Briefcase, Star, Activity, Swords, Skull, Package
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
  allied: 'Ingen',
  grade: 9,
  experience: '7 968 EP',
  capital: '770 sm',
  health: '118/118 KP',
  form: '110 %',
  time: '128 rundor',
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
</script>

<template>
  <div class="space-y-6">
    <h1 class="text-2xl font-bold" style="font-family: var(--font-display);">Min Gladiator</h1>

    <Tabs default-value="info">
      <!-- Scrollable tabs with icons -->
      <div class="tabs-scroll-container overflow-x-auto pb-1 -mx-1 px-1">
        <TabsList class="inline-flex gap-1 w-max">
          <TabsTrigger
            v-for="tab in tabs"
            :key="tab.value"
            :value="tab.value"
            class="text-xs sm:text-sm gap-1.5 shrink-0"
          >
            <component :is="tab.icon" class="size-3.5 hidden sm:block" />
            {{ tab.label }}
          </TabsTrigger>
        </TabsList>
      </div>

      <TabsContent value="info" class="mt-6 space-y-6">
        <!-- Gladiator Book - special gold accent -->
        <Card class="card-interactive card-gold-accent">
          <CardHeader class="pb-3">
            <CardDescription class="italic text-sm leading-relaxed border-l-2 border-primary/30 pl-3">
              I stadens gladiatorbok finns följande allmän information inskriven för
              <strong class="text-foreground">{{ gladiator.name }}</strong>.
              Stadens bokhållare Lorensis Sonius har själv signerat och verifierat att följande information är korrekt.
            </CardDescription>
          </CardHeader>
          <CardContent>
            <div class="grid gap-6 md:grid-cols-2">
              <!-- Left column: Stats -->
              <div class="space-y-4">
                <!-- Identity section -->
                <div class="space-y-2">
                  <h3 class="text-xs font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
                    Identitet
                  </h3>
                  <div class="grid grid-cols-[130px_1fr] gap-y-2.5 gap-x-3 text-sm">
                    <span class="text-muted-foreground">Profil</span>
                    <span>{{ gladiator.profile }}</span>

                    <span class="text-muted-foreground">Namn</span>
                    <span class="flex items-center gap-1.5">
                      <strong>{{ gladiator.name }}</strong>
                      <span v-if="gladiator.online" class="flex items-center gap-1">
                        <span class="relative flex size-2">
                          <span class="absolute inline-flex size-full animate-ping rounded-full bg-green-400 opacity-75" />
                          <span class="relative inline-flex size-2 rounded-full bg-green-500" />
                        </span>
                        <span class="text-xs text-green-600 dark:text-green-400">Online</span>
                      </span>
                    </span>

                    <span class="text-muted-foreground">Titel</span>
                    <span>{{ gladiator.title }}</span>

                    <span class="text-muted-foreground">Ranking</span>
                    <span class="tabular-nums">{{ gladiator.ranking }}</span>

                    <span class="text-muted-foreground">Rankingpoäng</span>
                    <span class="flex items-center gap-1 tabular-nums">
                      {{ gladiator.rankingPoints }}
                      <Tooltip>
                        <TooltipTrigger as-child>
                          <HelpCircle class="size-3 text-muted-foreground/50 cursor-help" />
                        </TooltipTrigger>
                        <TooltipContent>Rankingpoäng baserat på 6vs6 strider</TooltipContent>
                      </Tooltip>
                    </span>

                    <span class="text-muted-foreground">Max RP</span>
                    <span class="tabular-nums">{{ gladiator.maxRP }}</span>

                    <span class="text-muted-foreground">Rykte</span>
                    <span class="tabular-nums">{{ gladiator.reputation }}</span>

                    <span class="text-muted-foreground">Ryktestyp</span>
                    <span class="text-destructive font-medium">{{ gladiator.reputationType }}</span>
                  </div>
                </div>

                <Separator />

                <!-- Race/bio section -->
                <div class="space-y-2">
                  <h3 class="text-xs font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
                    Karaktär
                  </h3>
                  <div class="grid grid-cols-[130px_1fr] gap-y-2.5 gap-x-3 text-sm">
                    <span class="text-muted-foreground">Ras</span>
                    <span class="font-medium">{{ gladiator.race }}</span>

                    <span class="text-muted-foreground">Ålder</span>
                    <span>{{ gladiator.age }}</span>

                    <span class="text-muted-foreground">Kön</span>
                    <span>{{ gladiator.gender }}</span>
                  </div>
                </div>

                <Separator />

                <!-- Alliance section -->
                <div class="space-y-2">
                  <h3 class="text-xs font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
                    Klan & Allians
                  </h3>
                  <div class="grid grid-cols-[130px_1fr] gap-y-2.5 gap-x-3 text-sm">
                    <span class="text-muted-foreground">Klan</span>
                    <NuxtLink to="#" class="text-primary font-medium hover:underline underline-offset-2">{{ gladiator.clan }}</NuxtLink>

                    <span class="text-muted-foreground">Allierad med</span>
                    <span class="text-muted-foreground italic">{{ gladiator.allied }}</span>
                  </div>
                </div>

                <Separator />

                <!-- Stats section -->
                <div class="space-y-2">
                  <h3 class="text-xs font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
                    Status
                  </h3>
                  <div class="grid grid-cols-[130px_1fr] gap-y-2.5 gap-x-3 text-sm">
                    <span class="text-muted-foreground">Grad</span>
                    <span class="font-bold text-lg tabular-nums" style="font-family: var(--font-display);">{{ gladiator.grade }}</span>

                    <span class="text-muted-foreground">Erfarenhet</span>
                    <span class="tabular-nums">{{ gladiator.experience }}</span>

                    <span class="text-muted-foreground">Kapital</span>
                    <span class="tabular-nums">{{ gladiator.capital }}</span>
                  </div>
                </div>

                <Separator />

                <!-- Combat stats -->
                <div class="space-y-2">
                  <h3 class="text-xs font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
                    Strid
                  </h3>
                  <div class="grid grid-cols-[130px_1fr] gap-y-2.5 gap-x-3 text-sm">
                    <span class="text-muted-foreground">Hälsa</span>
                    <span class="tabular-nums font-medium">{{ gladiator.health }}</span>

                    <span class="text-muted-foreground">Form</span>
                    <span class="tabular-nums font-medium text-emerald-600 dark:text-emerald-400">{{ gladiator.form }}</span>

                    <span class="text-muted-foreground">Tid</span>
                    <span class="tabular-nums">{{ gladiator.time }}</span>
                  </div>
                </div>

                <Separator />

                <!-- Attributes - visual chips -->
                <div class="space-y-2">
                  <h3 class="text-xs font-bold uppercase tracking-wider text-muted-foreground" style="font-family: var(--font-display); font-size: 0.65rem;">
                    Grundegenskaper
                  </h3>
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

              <!-- Right column: Merits -->
              <div class="hidden md:block">
                <GameMeritSection />
              </div>
            </div>
          </CardContent>
        </Card>

        <!-- Merits on mobile -->
        <Card class="md:hidden card-interactive">
          <CardContent class="pt-6">
            <GameMeritSection />
          </CardContent>
        </Card>

        <!-- Tactics - Battles -->
        <Card class="card-interactive">
          <CardHeader class="pb-3">
            <CardTitle class="flex items-center gap-2 text-base" style="font-family: var(--font-display);">
              <Swords class="size-4 text-primary" />
              Standardtaktik för strider
            </CardTitle>
          </CardHeader>
          <CardContent class="space-y-4">
            <div class="grid gap-4 sm:grid-cols-2 lg:grid-cols-3">
              <div class="space-y-2">
                <label class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Taktik</label>
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
                <label class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Ge upp vid</label>
                <Select v-model="selectedGiveUp">
                  <SelectTrigger>
                    <SelectValue />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem v-for="opt in giveUpOptions" :key="opt" :value="opt">{{ opt }}</SelectItem>
                  </SelectContent>
                </Select>
              </div>

              <div class="space-y-2">
                <label class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Tillåt lagspel att starta vid</label>
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
            </div>

            <div class="flex items-center gap-2 pt-1">
              <Checkbox id="restore-tournament" v-model:checked="restoreAfterTournament" />
              <label for="restore-tournament" class="text-sm cursor-pointer">Återställ efter turneringsstrid</label>
            </div>
          </CardContent>
        </Card>

        <!-- Tactics - Beasts -->
        <Card class="card-interactive">
          <CardHeader class="pb-3">
            <CardTitle class="flex items-center gap-2 text-base" style="font-family: var(--font-display);">
              <Skull class="size-4 text-primary" />
              Standardtaktik för odjursstrider
            </CardTitle>
          </CardHeader>
          <CardContent>
            <div class="grid gap-4 sm:grid-cols-2">
              <div class="space-y-2">
                <label class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Taktik mot odjur</label>
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
                <label class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Ge upp vid (odjur)</label>
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
          </CardContent>
        </Card>

        <!-- Other Settings -->
        <Card class="card-interactive">
          <CardHeader class="pb-3">
            <CardTitle class="flex items-center gap-2 text-base" style="font-family: var(--font-display);">
              <Package class="size-4 text-primary" />
              Övriga inställningar
            </CardTitle>
          </CardHeader>
          <CardContent class="space-y-4">
            <div class="max-w-sm space-y-2">
              <label class="text-xs font-semibold uppercase tracking-wide text-muted-foreground" style="font-size: 0.65rem;">Tillåt ta emot föremål från</label>
              <Select model-value="Alla">
                <SelectTrigger>
                  <SelectValue />
                </SelectTrigger>
                <SelectContent>
                  <SelectItem value="Alla">Alla</SelectItem>
                  <SelectItem value="Klanmedlemmar">Klanmedlemmar</SelectItem>
                  <SelectItem value="Ingen">Ingen</SelectItem>
                </SelectContent>
              </Select>
            </div>

            <div class="flex items-center gap-2">
              <Checkbox id="hide-max" :checked="true" />
              <label for="hide-max" class="text-sm cursor-pointer">Dölj maxskada i statistik och ärans sal?</label>
            </div>
          </CardContent>
        </Card>
      </TabsContent>

      <!-- Placeholder content for other tabs -->
      <TabsContent v-for="tab in tabs.filter(t => t.value !== 'info')" :key="tab.value" :value="tab.value" class="mt-6">
        <Card class="card-interactive">
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
