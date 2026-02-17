<script setup lang="ts">
import {
  Zap, Sword, Trophy, ScrollText, Dices, Calendar, Target, ListChecks,
  TreePine, Skull, Map, Mountain, Snowflake, Bug, Castle,
  Users, UserCircle, Handshake, Shield, Flag, Heart, UsersRound,
  Hammer, Store, Gavel, Wrench, Search, Building, ChevronDown, X
} from 'lucide-vue-next'

interface NavItem {
  label: string
  icon: any
  href?: string
  badge?: number
  children?: NavItem[]
}

const navigation: { group: string; items: NavItem[] }[] = [
  {
    group: 'Genvägar',
    items: [
      { label: 'Massutmana', icon: Zap, href: '/arena/mass-challenge' },
      { label: 'Utmaningar Radera', icon: Target, href: '/arena/challenges/delete' },
      { label: 'Hälsans Sal', icon: Heart, href: '/city/healing' },
      { label: 'Mat', icon: TreePine, href: '/city/food' },
    ],
  },
  {
    group: 'Arenan',
    items: [
      { label: 'Lagspel', icon: Users, href: '/arena/team' },
      { label: 'Rankade lagspel', icon: Trophy, href: '/arena/ranked', children: [
        { label: 'Topplistor', icon: ScrollText, href: '/arena/rankings' },
      ]},
      { label: 'Slumpduell', icon: Dices, href: '/arena/random-duel' },
      { label: 'Turneringar', icon: Calendar, href: '/arena/tournaments', badge: 19, children: [
        { label: 'Tisdagskaos 14:00', icon: Calendar, href: '/arena/tournaments/tuesday' },
      ]},
      { label: 'Dagliga bataljer', icon: Sword, href: '/arena/daily-battles', badge: 1 },
      { label: 'Utmaningar', icon: Target, href: '/arena/challenges', badge: 26 },
      { label: 'Dagliga uppdrag', icon: ListChecks, href: '/arena/daily-quests' },
    ],
  },
  {
    group: 'Äventyr & Odjur',
    items: [
      { label: 'Odjur', icon: Skull, href: '/adventure/beasts' },
      { label: 'Äventyr', icon: Map, href: '/adventure/adventures' },
      { label: 'Isvalla', icon: Snowflake, href: '/adventure/isvalla' },
      { label: 'Milugs Monsterjakt', icon: Bug, href: '/adventure/milugs' },
      { label: 'Bergspasset', icon: Mountain, href: '/adventure/mountain-pass' },
      { label: 'Tusenåriga skogen', icon: TreePine, href: '/adventure/forest' },
    ],
  },
  {
    group: 'Gemenskap',
    items: [
      { label: 'Klaner', icon: Shield, href: '/community/clans' },
      { label: 'Sammanslutningar', icon: Handshake, href: '/community/alliances' },
      { label: 'Klanligan', icon: Trophy, href: '/community/clan-league' },
      { label: 'Andraligan', icon: Flag, href: '/community/second-league' },
      { label: 'Miniligan', icon: UsersRound, href: '/community/mini-league' },
      { label: 'Min gemenskap', icon: UserCircle, href: '/community/my', children: [
        { label: 'Strider', icon: Sword, href: '/community/my/battles' },
        { label: 'Byggnader', icon: Building, href: '/community/my/buildings' },
        { label: 'Vidunder', icon: Skull, href: '/community/my/monsters' },
      ]},
    ],
  },
  {
    group: 'Marknaden',
    items: [
      { label: 'Köpmän', icon: Store, href: '/market/merchants' },
      { label: 'Auktioner', icon: Gavel, href: '/market/auctions' },
      { label: 'Verkstad', icon: Wrench, href: '/market/workshop', children: [
        { label: 'Pågående arbeten', icon: Hammer, href: '/market/workshop/ongoing' },
      ]},
      { label: 'Sök utrustning', icon: Search, href: '/market/search' },
    ],
  },
  {
    group: 'Staden',
    items: [
      { label: 'Hälsans Sal', icon: Heart, href: '/city/healing' },
      { label: 'Yrkesgillet', icon: Castle, href: '/city/guild' },
    ],
  },
]

const openGroups = ref<string[]>(navigation.map(n => n.group))
const route = useRoute()
const emit = defineEmits<{ close: [] }>()

function toggleGroup(group: string) {
  const idx = openGroups.value.indexOf(group)
  if (idx >= 0) {
    openGroups.value.splice(idx, 1)
  } else {
    openGroups.value.push(group)
  }
}

function isActive(href?: string) {
  return href ? route.path === href : false
}
</script>

<template>
  <nav class="space-y-1 py-2">
    <div v-for="section in navigation" :key="section.group" class="px-2">
      <!-- Group header -->
      <button
        class="flex w-full items-center justify-between rounded-md px-2 py-1.5 text-muted-foreground hover:text-foreground transition-colors"
        @click="toggleGroup(section.group)"
      >
        <span class="text-[0.65rem] font-bold uppercase tracking-widest" style="font-family: var(--font-display);">
          {{ section.group }}
        </span>
        <ChevronDown
          class="size-3 transition-transform duration-200"
          :class="{ '-rotate-90': !openGroups.includes(section.group) }"
        />
      </button>

      <!-- Group items -->
      <div
        v-if="openGroups.includes(section.group)"
        class="mt-0.5 space-y-0.5"
      >
        <template v-for="item in section.items" :key="item.label">
          <NuxtLink
            :to="item.href || '#'"
            class="nav-item group flex items-center gap-2 rounded-md px-2 py-1.5 text-sm transition-colors"
            :class="isActive(item.href)
              ? 'bg-accent/60 text-accent-foreground font-medium border-l-2 border-primary'
              : 'hover:bg-accent/30'"
          >
            <component
              :is="item.icon"
              class="size-4 shrink-0 transition-colors"
              :class="isActive(item.href) ? 'text-primary' : 'text-muted-foreground group-hover:text-foreground'"
            />
            <span class="flex-1 truncate">{{ item.label }}</span>
            <!-- Notification badge -->
            <Badge
              v-if="item.badge"
              variant="secondary"
              class="badge-notify ml-auto h-5 min-w-5 justify-center rounded-full px-1.5 text-[0.6rem] font-bold bg-primary/15 text-primary"
            >
              {{ item.badge }}
            </Badge>
          </NuxtLink>

          <!-- Sub-items -->
          <div v-if="item.children && openGroups.includes(section.group)" class="ml-4 border-l border-border/50 space-y-0.5 pl-1">
            <NuxtLink
              v-for="child in item.children"
              :key="child.label"
              :to="child.href || '#'"
              class="nav-item flex items-center gap-2 rounded-md px-2 py-1 text-xs transition-colors"
              :class="isActive(child.href)
                ? 'bg-accent/60 text-accent-foreground font-medium'
                : 'text-muted-foreground hover:bg-accent/30 hover:text-foreground'"
            >
              <component :is="child.icon" class="size-3 shrink-0" />
              <span>{{ child.label }}</span>
            </NuxtLink>
          </div>
        </template>
      </div>
    </div>
  </nav>
</template>
