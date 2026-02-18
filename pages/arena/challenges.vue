<script setup lang="ts">
import { Eye, Swords, ShieldAlert, Send, Inbox, Filter, X } from 'lucide-vue-next'
import challengeHeroSrc from '~/assets/utmaningar2.png'

type ChallengeRow = {
  opponent: string
  grade: number
  race: string
  matchType: string
  health: string
  date: string
}

const tab = ref<'incoming' | 'outgoing'>('incoming')
const myGrade = ref('4')
const maxGrade = ref('100')
const selectedRace = ref('Alla')
const hideBotChallenges = ref(true)
const onlyReady = ref(true)

const incomingChallenges: ChallengeRow[] = [
  { opponent: 'Alven Dessala', grade: 7, race: 'Alv', matchType: 'Normal', health: 'Frisk', date: 'idag 06:03' },
  { opponent: 'Orken Goremace', grade: 7, race: 'Ork', matchType: 'Normal', health: 'Frisk', date: 'idag 02:03' },
  { opponent: 'Alven Sagebloom', grade: 9, race: 'Alv', matchType: 'Normal', health: 'Frisk', date: 'idag 16:03' },
  { opponent: 'Smaskarn', grade: 9, race: 'Alv', matchType: 'Normal', health: 'Frisk', date: '2026-02-16 07:48' },
  { opponent: 'Kesonir den Fege', grade: 9, race: 'Goblin', matchType: 'Normal', health: 'Frisk', date: 'idag 09:55' },
  { opponent: 'Garrolius Dux', grade: 9, race: 'Goblin', matchType: 'Normal', health: 'Frisk', date: 'igår 18:56' },
  { opponent: 'Picaña', grade: 11, race: 'Alv', matchType: 'Normal', health: 'Frisk', date: 'igår 22:59' },
]

const outgoingChallenges: ChallengeRow[] = [
  { opponent: 'Narzuk Blacktooth', grade: 10, race: 'Ork', matchType: 'Normal', health: 'Frisk', date: 'idag 08:11' },
  { opponent: 'Liathra Moonveil', grade: 12, race: 'Alv', matchType: 'Normal', health: 'Frisk', date: 'igår 21:19' },
  { opponent: 'VäktarDåren Cowen II', grade: 34, race: 'Dvärg', matchType: 'Normal', health: 'Frisk', date: 'igår 18:03' },
]

const rows = computed(() => (tab.value === 'incoming' ? incomingChallenges : outgoingChallenges))
</script>

<template>
  <div class="space-y-5">
    <section class="hero-shell rounded-xl p-4 lg:p-5">
      <div class="flex items-start gap-4">
        <div class="min-w-0 flex-1 space-y-3">
          <p class="section-kicker">Arenan</p>
          <h1 class="text-3xl font-bold tracking-tight lg:text-4xl" style="font-family: var(--font-display);">
            Utmaningar
          </h1>
          <p class="text-sm leading-relaxed text-foreground/85">
            För att lösa en skuld (eller värre), få folket i staden nöjda eller helt enkelt för att gladiatorer
            älskar att stå på arenans blodiga sand har de en vana att utmana varandra i dueller.
          </p>
          <p class="text-sm leading-relaxed text-foreground/85">
            Nedan kan du se dina mottagna och skickade utmaningar. När du antar en utmaning har du alltid möjlighet
            att välja taktik och när du skall ge upp. Du kan även ange att din gladiator för tillfället inte är redo
            vilket gör att andra gladiatorer inte kan anta dina skickade utmaningar, något som kan vara användbart om
            du t.ex står i kö för slumpduell.
          </p>
          <div class="flex flex-wrap gap-2">
            <div class="hero-stat-pill">
              <Inbox class="size-3.5 text-primary" />
              <span>{{ incomingChallenges.length }} mottagna</span>
            </div>
            <div class="hero-stat-pill">
              <Send class="size-3.5 text-primary" />
              <span>{{ outgoingChallenges.length }} skickade</span>
            </div>
          </div>
        </div>

        <div
          class="shrink-0 overflow-hidden rounded-lg border border-border/70 shadow-[0_8px_18px_oklch(0_0_0_/_0.1)]"
          style="width: 360px; height: 216px;"
        >
          <img
            :src="challengeHeroSrc"
            alt="Utmaningar"
            class="h-full w-full object-cover object-center"
            style="width: 100%; height: 100%; object-fit: cover;"
          />
        </div>
      </div>
    </section>

    <Card class="surface-card card-interactive">
      <CardContent class="space-y-4 pt-5">
        <div class="flex flex-wrap items-center gap-2">
          <Button
            size="sm"
            class="h-8"
            :variant="tab === 'incoming' ? 'default' : 'outline'"
            @click="tab = 'incoming'"
          >
            <Inbox class="mr-1 size-3.5" />
            Mottagna utmaningar
          </Button>
          <Button
            size="sm"
            class="h-8"
            :variant="tab === 'outgoing' ? 'default' : 'outline'"
            @click="tab = 'outgoing'"
          >
            <Send class="mr-1 size-3.5" />
            Skickade utmaningar
          </Button>

          <div class="ml-auto flex items-center gap-2 text-xs text-muted-foreground">
            <Filter class="size-3.5" />
            <button class="hover:text-foreground transition-colors">Markera 20 som lästa</button>
          </div>
        </div>

        <div class="grid gap-3 lg:grid-cols-[90px_110px_180px_1fr]">
          <div class="space-y-1.5">
            <label class="section-kicker">Min grad</label>
            <Input v-model="myGrade" />
          </div>
          <div class="space-y-1.5">
            <label class="section-kicker">Max grad</label>
            <Input v-model="maxGrade" />
          </div>
          <div class="space-y-1.5">
            <label class="section-kicker">Ras</label>
            <Select v-model="selectedRace">
              <SelectTrigger>
                <SelectValue />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="Alla">Alla</SelectItem>
                <SelectItem value="Alv">Alv</SelectItem>
                <SelectItem value="Ork">Ork</SelectItem>
                <SelectItem value="Dvärg">Dvärg</SelectItem>
                <SelectItem value="Goblin">Goblin</SelectItem>
                <SelectItem value="Människa">Människa</SelectItem>
              </SelectContent>
            </Select>
          </div>
          <div class="grid gap-2 sm:grid-cols-2 lg:pt-6">
            <div class="flex items-center gap-2">
              <Checkbox id="hide-bots" v-model:checked="hideBotChallenges" />
              <label for="hide-bots" class="text-sm cursor-pointer">Dölj utmaningar från bottar</label>
            </div>
            <div class="flex items-center gap-2">
              <Checkbox id="only-ready" v-model:checked="onlyReady" />
              <label for="only-ready" class="text-sm cursor-pointer">Enbart redo</label>
            </div>
          </div>
        </div>
      </CardContent>
    </Card>

    <Card class="surface-card card-interactive overflow-hidden">
      <CardContent class="p-0">
        <div class="hidden md:block">
          <table class="w-full text-sm">
            <thead class="bg-muted/55">
              <tr class="border-b">
                <th class="px-3 py-2 text-left font-semibold text-muted-foreground">Motståndare</th>
                <th class="px-3 py-2 text-left font-semibold text-muted-foreground">Grad</th>
                <th class="px-3 py-2 text-left font-semibold text-muted-foreground">Ras</th>
                <th class="px-3 py-2 text-left font-semibold text-muted-foreground">Matchtyp</th>
                <th class="px-3 py-2 text-left font-semibold text-muted-foreground">Hälsa</th>
                <th class="px-3 py-2 text-left font-semibold text-muted-foreground">Datum</th>
                <th class="px-3 py-2 text-right font-semibold text-muted-foreground">Åtgärd</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="row in rows"
                :key="`${tab}-${row.opponent}-${row.date}`"
                class="border-b border-border/55 transition-colors hover:bg-accent/20"
              >
                <td class="px-3 py-2.5">
                  <div class="inline-flex items-center gap-1.5">
                    <Eye class="size-3.5 text-muted-foreground" />
                    <a href="#" class="text-primary hover:underline underline-offset-2">{{ row.opponent }}</a>
                  </div>
                </td>
                <td class="px-3 py-2.5 tabular-nums">{{ row.grade }}</td>
                <td class="px-3 py-2.5">{{ row.race }}</td>
                <td class="px-3 py-2.5">{{ row.matchType }}</td>
                <td class="px-3 py-2.5">
                  <span class="inline-flex items-center gap-1 rounded-full bg-emerald-500/10 px-2 py-0.5 text-emerald-600 dark:text-emerald-400">
                    <ShieldAlert class="size-3" />
                    {{ row.health }}
                  </span>
                </td>
                <td class="px-3 py-2.5 text-muted-foreground">{{ row.date }}</td>
                <td class="px-3 py-2.5">
                  <div class="flex items-center justify-end gap-2">
                    <Button size="sm" class="h-7 px-2.5 text-xs">Acceptera</Button>
                    <Button size="icon" variant="ghost" class="size-7 text-muted-foreground hover:text-destructive">
                      <X class="size-3.5" />
                    </Button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="space-y-2 p-3 md:hidden">
          <div
            v-for="row in rows"
            :key="`mobile-${tab}-${row.opponent}-${row.date}`"
            class="rounded-lg border border-border/65 bg-card/55 p-3"
          >
            <div class="flex items-start justify-between gap-2">
              <a href="#" class="font-semibold text-primary hover:underline underline-offset-2">{{ row.opponent }}</a>
              <span class="text-xs text-muted-foreground">{{ row.date }}</span>
            </div>
            <div class="mt-2 grid grid-cols-2 gap-2 text-xs text-muted-foreground">
              <span>Grad: <strong class="text-foreground">{{ row.grade }}</strong></span>
              <span>Ras: <strong class="text-foreground">{{ row.race }}</strong></span>
              <span>Match: <strong class="text-foreground">{{ row.matchType }}</strong></span>
              <span>Hälsa: <strong class="text-emerald-600 dark:text-emerald-400">{{ row.health }}</strong></span>
            </div>
            <div class="mt-3 flex items-center justify-end gap-2">
              <Button size="sm" class="h-7 px-2.5 text-xs">Acceptera</Button>
              <Button size="icon" variant="ghost" class="size-7 text-muted-foreground hover:text-destructive">
                <X class="size-3.5" />
              </Button>
            </div>
          </div>
        </div>
      </CardContent>
    </Card>
  </div>
</template>
