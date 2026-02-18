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
  Settings2,
} from 'lucide-vue-next'

interface NavItem {
  label: string
  icon: any
  href?: string
  badge?: number
  children?: NavItem[]
}

type NavSection = { group: string; items: NavItem[] }
const navigation: NavSection[] = [
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
      { label: 'Dagliga uppdrag', icon: ListChecks, href: '/arena/daily-quests' },
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
const customizationMode = ref(false)
const hiddenItemKeys = ref<string[]>([])
const draftHiddenItemKeys = ref<string[]>([])
const STORAGE_KEY = 'lanista.navigation.hidden-items.v1'

onMounted(() => {
  const raw = localStorage.getItem(STORAGE_KEY)
  if (!raw) {
    return
  }
  try {
    const parsed = JSON.parse(raw)
    if (Array.isArray(parsed)) {
      hiddenItemKeys.value = parsed.filter((value): value is string => typeof value === 'string')
    }
  } catch {
    hiddenItemKeys.value = []
  }
})

function navKey(sectionGroup: string, item: NavItem, parentLabel?: string) {
  if (item.href) {
    return item.href
  }
  const parentPart = parentLabel ? `${parentLabel}>` : ''
  return `${sectionGroup}:${parentPart}${item.label}`
}

function isHidden(key: string, source: string[] = hiddenItemKeys.value) {
  return source.includes(key)
}

function sectionHasVisibleItems(section: NavSection) {
  if (customizationMode.value) {
    return true
  }
  return section.items.some((item) => !isHidden(navKey(section.group, item)))
}

function shouldRenderItem(sectionGroup: string, item: NavItem) {
  if (customizationMode.value) {
    return true
  }
  return !isHidden(navKey(sectionGroup, item))
}

function shouldRenderChild(sectionGroup: string, parent: NavItem, child: NavItem) {
  if (customizationMode.value) {
    return true
  }
  return !isHidden(navKey(sectionGroup, child, parent.label))
}

function isGroupOpen(group: string) {
  return customizationMode.value || openGroups.value.includes(group)
}

function toggleGroup(group: string) {
  if (customizationMode.value) {
    return
  }
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

function startCustomization() {
  draftHiddenItemKeys.value = [...hiddenItemKeys.value]
  customizationMode.value = true
  openGroups.value = navigation.map((section) => section.group)
}

function isDraftVisible(key: string) {
  return !draftHiddenItemKeys.value.includes(key)
}

function setDraftVisibility(key: string, checked: boolean) {
  const shouldShow = checked
  if (shouldShow) {
    draftHiddenItemKeys.value = draftHiddenItemKeys.value.filter((value) => value !== key)
    return
  }
  if (!draftHiddenItemKeys.value.includes(key)) {
    draftHiddenItemKeys.value.push(key)
  }
}

function onDraftCheckboxChange(key: string, event: Event) {
  const target = event.target as HTMLInputElement | null
  setDraftVisibility(key, Boolean(target?.checked))
}

function saveCustomization() {
  hiddenItemKeys.value = [...new Set(draftHiddenItemKeys.value)]
  localStorage.setItem(STORAGE_KEY, JSON.stringify(hiddenItemKeys.value))
  customizationMode.value = false
}

function cancelCustomization() {
  customizationMode.value = false
  draftHiddenItemKeys.value = []
}
</script>

<template>
  <nav class="space-y-2 py-3">
    <div v-for="section in navigation" :key="section.group" v-show="sectionHasVisibleItems(section)" class="px-2.5">
      <button
        class="group flex w-full items-center justify-between rounded-lg border border-transparent px-2.5 py-2 text-muted-foreground hover:border-border/60 hover:bg-card/70 hover:text-foreground transition-all"
        @click="toggleGroup(section.group)"
      >
        <span class="section-kicker text-[0.62rem]">{{ section.group }}</span>
        <ChevronDown
          class="size-3.5 transition-transform duration-200 group-hover:text-primary"
          :class="{ '-rotate-90': !isGroupOpen(section.group) }"
        />
      </button>

      <div
        v-if="isGroupOpen(section.group)"
        class="mt-1 space-y-1 rounded-lg border border-border/40 bg-card/35 p-1.5 shadow-[0_6px_14px_oklch(0_0_0_/_0.08)] dark:shadow-[0_10px_18px_oklch(0_0_0_/_0.35)]"
      >
        <template v-for="item in section.items" :key="item.label">
          <NuxtLink
            v-if="shouldRenderItem(section.group, item) && !customizationMode"
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

          <div
            v-else-if="shouldRenderItem(section.group, item)"
            class="flex items-center gap-2 rounded-md border border-border/50 bg-card/65 px-2.5 py-2 text-sm text-foreground"
          >
            <component :is="item.icon" class="size-4 shrink-0 text-primary" />
            <span class="flex-1 truncate">{{ item.label }}</span>
            <input
              type="checkbox"
              class="size-4 cursor-pointer accent-[oklch(0.66_0.10_80)]"
              :checked="isDraftVisible(navKey(section.group, item))"
              @change="onDraftCheckboxChange(navKey(section.group, item), $event)"
            >
          </div>

          <div
            v-if="item.children && (customizationMode || shouldRenderItem(section.group, item))"
            class="ml-4 border-l border-border/70 space-y-1 pl-2"
          >
            <NuxtLink
              v-for="child in item.children"
              v-show="shouldRenderChild(section.group, item, child) && !customizationMode"
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

            <div
              v-for="child in item.children"
              v-show="customizationMode"
              :key="`${item.label}-${child.label}`"
              class="flex items-center gap-2 rounded-md border border-border/45 bg-card/55 px-2 py-1.5 text-xs text-foreground"
            >
              <component :is="child.icon" class="size-3.5 shrink-0 text-primary" />
              <span class="flex-1 truncate">{{ child.label }}</span>
              <input
                type="checkbox"
                class="size-4 cursor-pointer accent-[oklch(0.66_0.10_80)]"
                :checked="isDraftVisible(navKey(section.group, child, item.label))"
                @change="onDraftCheckboxChange(navKey(section.group, child, item.label), $event)"
              >
            </div>
          </div>
        </template>
      </div>
    </div>

    <div class="px-2.5 pt-1">
      <div class="rounded-lg border border-border/40 bg-card/45 p-2">
        <button
          v-if="!customizationMode"
          class="nav-item flex w-full items-center gap-2 rounded-md px-2.5 py-2 text-sm text-foreground/85 hover:bg-accent/35 hover:text-foreground transition-all"
          @click="startCustomization"
        >
          <Settings2 class="size-4 text-primary" />
          <span class="font-medium">Anpassa</span>
        </button>

        <div v-else class="space-y-2">
          <p class="text-xs text-muted-foreground">Alla val visas tillfälligt. Bocka i/ur och spara.</p>
          <div class="grid grid-cols-2 gap-2">
            <Button size="sm" class="h-8" @click="saveCustomization">Spara</Button>
            <Button size="sm" variant="outline" class="h-8" @click="cancelCustomization">Avbryt</Button>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>
