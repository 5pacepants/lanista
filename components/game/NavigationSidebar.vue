<script setup lang="ts">
import {
  Zap,
  Target,
  Heart,
  TreePine,
  Users,
  Trophy,
  ScrollText,
  Dices,
  Calendar,
  Sword,
  ListChecks,
  Skull,
  Map,
  Snowflake,
  Bug,
  Mountain,
  Shield,
  Handshake,
  Flag,
  UsersRound,
  UserCircle,
  Building,
  Store,
  Gavel,
  Wrench,
  Hammer,
  Search,
  Swords,
  Crown,
  Castle,
  Briefcase,
  Beer,
  BookOpen,
  Banknote,
  LockKeyhole,
  Landmark,
  Vault,
  Newspaper,
  HelpCircle,
  ChevronDown,
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
      {
        label: 'Rankade lagspel',
        icon: Trophy,
        href: '/arena/ranked',
        children: [{ label: 'Topplistor', icon: ScrollText, href: '/arena/rankings' }],
      },
      { label: 'Slumpduell', icon: Dices, href: '/arena/random-duel' },
      { label: 'Turneringar', icon: Calendar, href: '/arena/tournaments', badge: 1 },
      { label: 'Dagliga bataljer', icon: Sword, href: '/arena/daily-battles', badge: 17 },
      { label: 'Utmaningar', icon: Target, href: '/arena/challenges', badge: 30 },
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
      {
        label: 'Min gemenskap',
        icon: UserCircle,
        href: '/community/my',
        children: [
          { label: 'Strider', icon: Swords, href: '/community/my/battles' },
          { label: 'Byggnader', icon: Building, href: '/community/my/buildings' },
          { label: 'Vidunder', icon: Skull, href: '/community/my/monsters' },
        ],
      },
    ],
  },
  {
    group: 'Marknaden',
    items: [
      { label: 'Köpmän', icon: Store, href: '/market/merchants' },
      { label: 'Auktioner', icon: Gavel, href: '/market/auctions' },
      {
        label: 'Verkstad',
        icon: Wrench,
        href: '/market/workshop',
        children: [{ label: 'Pågående arbeten', icon: Hammer, href: '/market/workshop/ongoing' }],
      },
      { label: 'Sök utrustning', icon: Search, href: '/market/search' },
    ],
  },
  {
    group: 'Staden',
    items: [
      { label: 'Gladiatorer', icon: Users, href: '/city/gladiators' },
      { label: 'Blodfejder', icon: Swords, href: '/city/blood-feuds' },
      { label: 'Ärans Sal', icon: Crown, href: '/city/hall-of-honor' },
      { label: 'Stadsbyggnader', icon: Building, href: '/city/buildings' },
      { label: 'Jobbdistriktet', icon: Briefcase, href: '/city/jobs' },
      { label: 'Värdshuset', icon: Beer, href: '/city/tavern' },
      {
        label: 'Berättelser',
        icon: BookOpen,
        href: '/city/stories',
        children: [{ label: 'Blå murens arkiv', icon: ScrollText, href: '/city/stories/blue-wall-archive' }],
      },
      { label: 'Bank & Förråd', icon: Banknote, href: '/city/bank-storage' },
      { label: 'Kejsarens skattkammare', icon: Landmark, href: '/city/emperors-treasury' },
      { label: 'Kejsarens förråd', icon: Vault, href: '/city/emperors-storage' },
      { label: 'Fängelsehålan', icon: LockKeyhole, href: '/city/dungeon' },
      {
        label: 'Kyrkogården',
        icon: Castle,
        href: '/city/graveyard',
        children: [{ label: 'Legendernas ek', icon: TreePine, href: '/city/graveyard/legends-oak' }],
      },
      { label: 'Anslagstavlan', icon: Newspaper, href: '/city/bulletin-board' },
      { label: 'Hjälp', icon: HelpCircle, href: '/help' },
    ],
  },
]

const route = useRoute()
const openGroups = ref<string[]>(['Genvägar', 'Arenan'])

function toggleGroup(group: string) {
  const idx = openGroups.value.indexOf(group)
  if (idx >= 0) {
    openGroups.value.splice(idx, 1)
    return
  }
  openGroups.value.push(group)
}

function isActive(href?: string) {
  return href ? route.path === href : false
}
</script>

<template>
  <nav class="space-y-2 py-3">
    <div v-for="section in navigation" :key="section.group" class="px-2.5">
      <button
        class="group flex w-full items-center justify-between rounded-lg border border-transparent px-2.5 py-2 text-muted-foreground hover:border-border/60 hover:bg-card/70 hover:text-foreground transition-all"
        @click="toggleGroup(section.group)"
      >
        <span class="section-kicker text-[0.62rem]">{{ section.group }}</span>
        <ChevronDown
          class="size-3.5 transition-transform duration-200 group-hover:text-primary"
          :class="{ '-rotate-90': !openGroups.includes(section.group) }"
        />
      </button>

      <div
        v-if="openGroups.includes(section.group)"
        class="mt-1 space-y-1 rounded-lg border border-border/40 bg-card/35 p-1.5 shadow-[0_6px_14px_oklch(0_0_0_/_0.08)] dark:shadow-[0_10px_18px_oklch(0_0_0_/_0.35)]"
      >
        <template v-for="item in section.items" :key="item.label">
          <NuxtLink
            :to="item.href || '#'"
            class="nav-item nav-command group flex items-center gap-2 rounded-md px-2.5 py-2 text-sm transition-all"
            :class="isActive(item.href)
              ? 'bg-accent/70 text-accent-foreground font-semibold ring-1 ring-primary/30'
              : 'text-foreground/80 hover:bg-accent/35 hover:text-foreground'"
          >
            <component
              :is="item.icon"
              class="size-4 shrink-0 transition-colors"
              :class="isActive(item.href) ? 'text-primary' : 'text-muted-foreground group-hover:text-foreground'"
            />
            <span class="flex-1 truncate">{{ item.label }}</span>
            <Badge
              v-if="item.badge"
              variant="secondary"
              class="badge-notify ml-auto h-5 min-w-5 justify-center rounded-full px-1.5 text-[0.62rem] font-bold bg-primary/20 text-primary border border-primary/30"
            >
              {{ item.badge }}
            </Badge>
          </NuxtLink>

          <div v-if="item.children" class="ml-4 border-l border-border/70 space-y-1 pl-2">
            <NuxtLink
              v-for="child in item.children"
              :key="child.label"
              :to="child.href || '#'"
              class="nav-item flex items-center gap-2 rounded-md px-2 py-1.5 text-xs transition-all"
              :class="isActive(child.href)
                ? 'bg-accent/60 text-accent-foreground font-semibold ring-1 ring-primary/20'
                : 'text-muted-foreground hover:bg-accent/30 hover:text-foreground'"
            >
              <component :is="child.icon" class="size-3.5 shrink-0" />
              <span>{{ child.label }}</span>
            </NuxtLink>
          </div>
        </template>
      </div>
    </div>
  </nav>
</template>
