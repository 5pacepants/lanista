<script setup lang="ts">
import {
  Axe,
  Boxes,
  Check,
  Clock3,
  Crosshair,
  Gem,
  Hand,
  Info,
  Medal,
  ScrollText,
  Shield,
  Shirt,
  Sparkles,
  Swords,
  TreePine,
  FlaskConical,
  X,
} from 'lucide-vue-next'
import gladiatorSrc from '~/assets/gladiator.png'
import smidesringSrc from '~/assets/smidesring.jpg'
import tvaBiladFascesSrc from '~/assets/tvåbilad-fasces.png'
import narrensVantarSrc from '~/assets/narrens-vantar.jpg'

type SlotId =
  | 'twoHand'
  | 'ranged'
  | 'head'
  | 'shoulders'
  | 'chest'
  | 'hands'
  | 'legs'
  | 'feet'
  | 'cloak'
  | 'necklace'
  | 'ring'
  | 'amulet'
  | 'bracelet'
  | 'ornament'

type ItemCategory = 'Vapen' | 'Rustning' | 'Foremal' | 'Besvarjelser' | 'Material' | 'Mixturer'

type FactRow = { label: string; value: string }
type RequirementRow = { ok: boolean; title: string; detail: string; note?: string }

type EquipmentItem = {
  id: string
  name: string
  category: ItemCategory
  icon: any
  slotTargets: SlotId[]
  image?: string
  description: string
  facts: FactRow[]
  spellFacts: FactRow[]
  requirements: RequirementRow[]
  modifications: string[]
}

type SlotDef = {
  id: SlotId
  label: string
  x: string
  y: string
}

const slots: SlotDef[] = [
  { id: 'head', label: 'Huvud', x: '50%', y: '6%' },
  { id: 'shoulders', label: 'Axlar', x: '12%', y: '20%' },
  { id: 'chest', label: 'Harnesk', x: '88%', y: '20%' },
  { id: 'hands', label: 'Hander', x: '12%', y: '34%' },
  { id: 'twoHand', label: 'Tvahand', x: '88%', y: '34%' },
  { id: 'ranged', label: 'Distans', x: '12%', y: '48%' },
  { id: 'cloak', label: 'Mantel', x: '88%', y: '48%' },
  { id: 'necklace', label: 'Halsband', x: '12%', y: '62%' },
  { id: 'ring', label: 'Ring', x: '88%', y: '62%' },
  { id: 'amulet', label: 'Amulett', x: '12%', y: '76%' },
  { id: 'bracelet', label: 'Armband', x: '88%', y: '76%' },
  { id: 'ornament', label: 'Ornament', x: '24%', y: '92%' },
  { id: 'legs', label: 'Ben', x: '50%', y: '92%' },
  { id: 'feet', label: 'Fotter', x: '76%', y: '92%' },
]

function createDefaultDetails(name: string): Pick<EquipmentItem, 'description' | 'facts' | 'spellFacts' | 'requirements' | 'modifications'> {
  return {
    description: `${name} ar ett foremal i din utrustning. Det kan anvandas i ratt slot och bidra med bonusar beroende pa anvandning och bygge.`,
    facts: [
      { label: 'Typ', value: 'Utrustning' },
      { label: 'Skada', value: '-' },
      { label: 'PT-potential', value: '+0%' },
      { label: 'Typ av perfekt traff', value: '-' },
      { label: 'Handlingar', value: '0' },
      { label: 'Vikt', value: '1 KV' },
      { label: 'Brytvarde', value: '-' },
    ],
    spellFacts: [
      { label: 'Besvarjelsetyper', value: 'Inga' },
      { label: 'Max antal besvarjelser', value: '0' },
    ],
    requirements: [
      { ok: true, title: 'Krav uppfyllt', detail: 'Din gladiator uppfyller grundkraven for att anvanda foremalet.' },
    ],
    modifications: ['Inga modifikationer.'],
  }
}

const items: EquipmentItem[] = [
  {
    id: 'tvabilad-fasces',
    name: 'Tvabilad fasces',
    category: 'Vapen',
    icon: Axe,
    slotTargets: ['twoHand'],
    image: tvaBiladFascesSrc,
    description: 'Ett tungt tvahandsvapen byggt for brutala oppningar och hogt brytvarde i langa dueller.',
    facts: [
      { label: 'Typ', value: 'Yxa - Tvahand' },
      { label: 'Skada', value: '16 - 20 | 42' },
      { label: 'PT-potential', value: '+6%' },
      { label: 'Typ av perfekt traff', value: 'Krossande' },
      { label: 'Handlingar', value: '1 + 1' },
      { label: 'Vikt', value: '12 KV' },
      { label: 'Brytvarde', value: '104 (104 + 0)' },
    ],
    spellFacts: [
      { label: 'Besvarjelsetyper', value: 'Onyx, Smaragd' },
      { label: 'Max antal besvarjelser', value: '1' },
    ],
    requirements: [
      { ok: true, title: 'Krav: grad 10', detail: 'Din gladiator ar grad 11.' },
      { ok: true, title: 'Krav: Styrka 92', detail: 'Din gladiator har 95 i Styrka.' },
      { ok: false, title: 'Rek: Vapenskicklighet Yxa 85', detail: 'Nuvarande varde: 80.', note: 'Lagre varde ger fler fumel.' },
    ],
    modifications: ['+4 i vapenfardigheten Yxa', '+8 i egenskapen Undvika anfall'],
  },
  {
    id: 'bastardsvard',
    name: 'Bastardsvard',
    category: 'Vapen',
    icon: Swords,
    slotTargets: ['twoHand'],
    description: 'Detta svard har fatt sitt namn pa grund av att det ar langre an ett enhandssvard och kortare an ett tvahandssvard, alltsa en bastard mellan de tva.',
    facts: [
      { label: 'Typ', value: 'Svard - Tvahand' },
      { label: 'Skada', value: '13 - 17 | 38' },
      { label: 'PT-potential', value: '+7%' },
      { label: 'Typ av perfekt traff', value: 'Oundviklig' },
      { label: 'Handlingar', value: '1 + 1' },
      { label: 'Vikt', value: '11 KV' },
      { label: 'Brytvarde', value: '90 (90 + 0)' },
    ],
    spellFacts: [
      { label: 'Besvarjelsetyper', value: 'Prismatisk, Topas, Onyx, Safir, Smaragd, Exotisk' },
      { label: 'Max antal besvarjelser', value: '2' },
    ],
    requirements: [
      { ok: true, title: 'Krav: minst grad 10', detail: 'Din gladiator ar grad 11.' },
      { ok: true, title: 'Krav: minst 90 i Styrka', detail: 'Nuvarande varde: 95.' },
      { ok: true, title: 'Rek: minst 75 i vapenfardigheten Svard', detail: 'Nuvarande varde: 80.' },
      { ok: true, title: 'Notis om fummel', detail: 'Vardet baseras pa att din gladiator ar nastan latt och anvander normal taktik.' },
    ],
    modifications: ['+5 i vapenfardigheten Svard', '+10 i egenskapen Undvika anfall'],
  },
  {
    id: 'liten-sten',
    name: 'Liten sten',
    category: 'Vapen',
    icon: Crosshair,
    slotTargets: ['ranged'],
    ...createDefaultDetails('Liten sten'),
  },
  {
    id: 'narrens-vantar',
    name: 'Narrens vantar',
    category: 'Rustning',
    icon: Hand,
    slotTargets: ['hands'],
    image: narrensVantarSrc,
    ...createDefaultDetails('Narrens vantar'),
  },
  {
    id: 'jernharnesk',
    name: 'Jarnharnesk',
    category: 'Rustning',
    icon: Shirt,
    slotTargets: ['chest'],
    ...createDefaultDetails('Jarnharnesk'),
  },
  {
    id: 'smidesring',
    name: 'Smidesring',
    category: 'Foremal',
    icon: Gem,
    slotTargets: ['ring'],
    image: smidesringSrc,
    ...createDefaultDetails('Smidesring'),
  },
  {
    id: 'silverring',
    name: 'Silverring',
    category: 'Foremal',
    icon: Gem,
    slotTargets: ['ring'],
    image: smidesringSrc,
    ...createDefaultDetails('Silverring'),
  },
  {
    id: 'liraks-kallblodskedja',
    name: 'Liraks kallblodskedja',
    category: 'Foremal',
    icon: Medal,
    slotTargets: ['necklace'],
    ...createDefaultDetails('Liraks kallblodskedja'),
  },
  {
    id: 'kallblodsamulett',
    name: 'Kallblodsamulett',
    category: 'Foremal',
    icon: Shield,
    slotTargets: ['amulet'],
    ...createDefaultDetails('Kallblodsamulett'),
  },
  {
    id: 'ond-rot',
    name: 'En ond rot',
    category: 'Foremal',
    icon: TreePine,
    slotTargets: ['ornament'],
    ...createDefaultDetails('En ond rot'),
  },
  {
    id: 'forstyrkt-runa',
    name: 'Forstarkt runa',
    category: 'Besvarjelser',
    icon: ScrollText,
    slotTargets: [],
    ...createDefaultDetails('Forstarkt runa'),
  },
  {
    id: 'jarn',
    name: 'Jarn (4 st)',
    category: 'Material',
    icon: Boxes,
    slotTargets: [],
    ...createDefaultDetails('Jarn'),
  },
  {
    id: 'lader',
    name: 'Lader (1 st)',
    category: 'Material',
    icon: Boxes,
    slotTargets: [],
    ...createDefaultDetails('Lader'),
  },
  {
    id: 'silverdryck',
    name: 'Silverdryck',
    category: 'Mixturer',
    icon: FlaskConical,
    slotTargets: [],
    ...createDefaultDetails('Silverdryck'),
  },
]

const equippedBySlot = ref<Record<SlotId, string | null>>({
  twoHand: 'tvabilad-fasces',
  ranged: null,
  head: null,
  shoulders: null,
  chest: null,
  hands: 'narrens-vantar',
  legs: null,
  feet: null,
  cloak: null,
  necklace: null,
  ring: 'smidesring',
  amulet: null,
  bracelet: null,
  ornament: null,
})

const hideActiveMixtures = ref(false)
const selectedItemId = ref<string | null>(null)

const itemById = computed(() => new Map(items.map((item) => [item.id, item])))

const selectedItem = computed(() => (selectedItemId.value ? itemById.value.get(selectedItemId.value) ?? null : null))

const selectedItemComparison = computed(() => {
  if (!selectedItem.value || selectedItem.value.slotTargets.length === 0) {
    return 'Ingen slotjamforelse tillganglig'
  }
  const primarySlot = selectedItem.value.slotTargets[0]
  const equippedId = equippedBySlot.value[primarySlot]
  const equipped = equippedId ? itemById.value.get(equippedId) : null
  const slotLabel = slots.find((slot) => slot.id === primarySlot)?.label ?? 'Slot'
  if (!equipped) {
    return `${slotLabel}: Tom`
  }
  const damageFact = equipped.facts.find((fact) => fact.label === 'Skada')
  return `${slotLabel}: ${equipped.name}${damageFact ? ` (${damageFact.value})` : ''}`
})

const groupedInventory = computed(() => {
  const equippedIds = new Set(
    Object.values(equippedBySlot.value).filter((value): value is string => Boolean(value)),
  )

  return {
    Vapen: items.filter((item) => item.category === 'Vapen' && !equippedIds.has(item.id)),
    Rustning: items.filter((item) => item.category === 'Rustning' && !equippedIds.has(item.id)),
    Foremal: items.filter((item) => item.category === 'Foremal' && !equippedIds.has(item.id)),
    Besvarjelser: items.filter((item) => item.category === 'Besvarjelser'),
    Material: items.filter((item) => item.category === 'Material'),
    Mixturer: items.filter((item) => item.category === 'Mixturer'),
  }
})

function getEquippedItem(slotId: SlotId) {
  const itemId = equippedBySlot.value[slotId]
  if (!itemId) {
    return null
  }
  return itemById.value.get(itemId) || null
}

function findSlotForItem(itemId: string) {
  return (Object.keys(equippedBySlot.value) as SlotId[]).find((slotId) => equippedBySlot.value[slotId] === itemId)
}

function canDropInSlot(itemId: string, slotId: SlotId) {
  const item = itemById.value.get(itemId)
  return Boolean(item && item.slotTargets.includes(slotId))
}

function moveItemToSlot(itemId: string, slotId: SlotId) {
  if (!canDropInSlot(itemId, slotId)) {
    return
  }
  const sourceSlot = findSlotForItem(itemId)
  const targetItemId = equippedBySlot.value[slotId]
  if (sourceSlot === slotId) {
    return
  }
  if (sourceSlot) {
    equippedBySlot.value[sourceSlot] = targetItemId || null
  }
  equippedBySlot.value[slotId] = itemId
}

function onDragStart(event: DragEvent, itemId: string) {
  if (!event.dataTransfer) {
    return
  }
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('text/plain', itemId)
}

function onDrop(event: DragEvent, slotId: SlotId) {
  event.preventDefault()
  const itemId = event.dataTransfer?.getData('text/plain')
  if (!itemId) {
    return
  }
  moveItemToSlot(itemId, slotId)
}

function onDragOver(event: DragEvent) {
  event.preventDefault()
}

function openItemDetails(itemId: string) {
  selectedItemId.value = itemId
}

function closeItemDetails() {
  selectedItemId.value = null
}
</script>

<template>
  <div class="space-y-5">
    <Card class="surface-card-highlight card-interactive">
      <CardHeader class="pb-2">
        <CardTitle class="text-lg" style="font-family: var(--font-display);">Utrustning</CardTitle>
        <CardDescription>Dra utrustning till sloten for att byta utrustning.</CardDescription>
      </CardHeader>
      <CardContent>
        <div class="grid gap-4 xl:grid-cols-[230px_1fr_260px]">
          <Card class="surface-card h-fit">
            <CardHeader class="pb-2">
              <CardTitle class="text-sm">Aktiva mixturer</CardTitle>
            </CardHeader>
            <CardContent class="space-y-3 text-sm">
              <div v-if="!hideActiveMixtures" class="rounded-md border border-border/70 bg-background/55 px-3 py-2">
                <div class="flex items-center gap-2">
                  <FlaskConical class="size-4 text-primary" />
                  <span class="font-medium">Gyllene dryck</span>
                </div>
                <p class="mt-1 text-xs text-muted-foreground">Duration: 1h 20 min</p>
              </div>
              <div class="flex items-center gap-2">
                <input id="hide-active-mixtures" v-model="hideActiveMixtures" type="checkbox" class="size-4 accent-[oklch(0.66_0.10_80)]">
                <label for="hide-active-mixtures" class="cursor-pointer text-xs text-muted-foreground">Dolj aktiva mixturer</label>
              </div>
            </CardContent>
          </Card>

          <div class="relative mx-auto h-[760px] w-full max-w-[760px] rounded-xl border border-border/70 bg-card/40 p-4">
            <div class="absolute inset-4 rounded-xl border border-border/70 bg-background/60" />
            <div class="absolute inset-y-8 left-1/2 w-[420px] -translate-x-1/2 rounded-lg border border-border/70 bg-card/80 shadow-sm" />

            <img
              :src="gladiatorSrc"
              alt="Gladiator"
              class="pointer-events-none absolute left-1/2 top-1/2 z-10 h-[700px] w-auto -translate-x-1/2 -translate-y-1/2 object-contain"
            >

            <div
              v-for="slot in slots"
              :key="slot.id"
              class="absolute z-20 -translate-x-1/2 -translate-y-1/2"
              :style="{ left: slot.x, top: slot.y }"
            >
              <div
                class="group flex h-20 w-20 flex-col items-center justify-center rounded-lg border border-border/80 bg-card/90 p-1 text-center shadow-sm transition-colors hover:border-primary/60 hover:bg-card"
                @drop="onDrop($event, slot.id)"
                @dragover="onDragOver"
              >
                <template v-if="getEquippedItem(slot.id)">
                  <button
                    class="w-full"
                    type="button"
                    @click.stop="openItemDetails(getEquippedItem(slot.id)!.id)"
                  >
                    <img
                      v-if="getEquippedItem(slot.id)?.image"
                      :src="getEquippedItem(slot.id)?.image"
                      :alt="getEquippedItem(slot.id)?.name"
                      class="mx-auto h-12 w-12 rounded object-cover"
                      draggable="true"
                      @dragstart="onDragStart($event, getEquippedItem(slot.id)!.id)"
                    >
                    <div
                      v-else
                      class="mx-auto flex h-12 w-12 items-center justify-center rounded bg-muted text-muted-foreground"
                      draggable="true"
                      @dragstart="onDragStart($event, getEquippedItem(slot.id)!.id)"
                    >
                      <component :is="getEquippedItem(slot.id)?.icon" class="size-5" />
                    </div>
                    <span class="line-clamp-1 mt-1 block w-full text-[10px] font-medium leading-tight">
                      {{ getEquippedItem(slot.id)?.name }}
                    </span>
                  </button>
                </template>
                <template v-else>
                  <Sparkles class="size-4 text-muted-foreground/50" />
                  <span class="mt-1 text-[10px] text-muted-foreground">{{ slot.label }}</span>
                </template>
              </div>
            </div>
          </div>

          <Card class="surface-card h-fit">
            <CardHeader class="pb-2">
              <CardTitle class="text-sm">Information</CardTitle>
            </CardHeader>
            <CardContent class="space-y-2 text-sm">
              <p>Total vikt: <span class="font-semibold">Latt 15KV</span></p>
              <p>Faktisk Vikt: <span class="font-semibold">Latt 13KV</span></p>
              <p>Totalt skydd: <span class="font-semibold">3</span></p>
              <p>Perfekt Traff: <span class="font-semibold">1.91%</span></p>
              <p>PT-Potential: <span class="font-semibold">4.00%</span></p>
            </CardContent>
          </Card>
        </div>
      </CardContent>
    </Card>

    <div class="grid gap-4 lg:grid-cols-3">
      <Card class="surface-card card-interactive">
        <CardHeader class="pb-2"><CardTitle class="text-base">Vapen</CardTitle></CardHeader>
        <CardContent class="space-y-2">
          <button
            v-for="item in groupedInventory.Vapen"
            :key="item.id"
            type="button"
            class="flex w-full cursor-grab items-center gap-2 rounded-md border border-border/70 bg-background/60 px-2.5 py-2 text-left active:cursor-grabbing"
            draggable="true"
            @click="openItemDetails(item.id)"
            @dragstart="onDragStart($event, item.id)"
          >
            <component :is="item.icon" class="size-4 text-primary" />
            <span class="flex-1 text-sm">{{ item.name }}</span>
          </button>
        </CardContent>
      </Card>

      <Card class="surface-card card-interactive">
        <CardHeader class="pb-2"><CardTitle class="text-base">Rustning</CardTitle></CardHeader>
        <CardContent class="space-y-2">
          <button
            v-for="item in groupedInventory.Rustning"
            :key="item.id"
            type="button"
            class="flex w-full cursor-grab items-center gap-2 rounded-md border border-border/70 bg-background/60 px-2.5 py-2 text-left active:cursor-grabbing"
            draggable="true"
            @click="openItemDetails(item.id)"
            @dragstart="onDragStart($event, item.id)"
          >
            <component :is="item.icon" class="size-4 text-primary" />
            <span class="flex-1 text-sm">{{ item.name }}</span>
          </button>
        </CardContent>
      </Card>

      <Card class="surface-card card-interactive">
        <CardHeader class="pb-2"><CardTitle class="text-base">Foremal</CardTitle></CardHeader>
        <CardContent class="space-y-2">
          <button
            v-for="item in groupedInventory.Foremal"
            :key="item.id"
            type="button"
            class="flex w-full cursor-grab items-center gap-2 rounded-md border border-border/70 bg-background/60 px-2.5 py-2 text-left active:cursor-grabbing"
            draggable="true"
            @click="openItemDetails(item.id)"
            @dragstart="onDragStart($event, item.id)"
          >
            <component :is="item.icon" class="size-4 text-primary" />
            <span class="flex-1 text-sm">{{ item.name }}</span>
          </button>
        </CardContent>
      </Card>

      <Card class="surface-card card-interactive">
        <CardHeader class="pb-2"><CardTitle class="text-base">Besvarjelser</CardTitle></CardHeader>
        <CardContent class="space-y-2">
          <button
            v-for="item in groupedInventory.Besvarjelser"
            :key="item.id"
            type="button"
            class="flex w-full items-center gap-2 rounded-md border border-border/70 bg-background/60 px-2.5 py-2 text-left"
            @click="openItemDetails(item.id)"
          >
            <component :is="item.icon" class="size-4 text-primary" />
            <span class="flex-1 text-sm">{{ item.name }}</span>
          </button>
        </CardContent>
      </Card>

      <Card class="surface-card card-interactive">
        <CardHeader class="pb-2"><CardTitle class="text-base">Material</CardTitle></CardHeader>
        <CardContent class="space-y-2">
          <button
            v-for="item in groupedInventory.Material"
            :key="item.id"
            type="button"
            class="flex w-full items-center gap-2 rounded-md border border-border/70 bg-background/60 px-2.5 py-2 text-left"
            @click="openItemDetails(item.id)"
          >
            <component :is="item.icon" class="size-4 text-primary" />
            <span class="flex-1 text-sm">{{ item.name }}</span>
          </button>
        </CardContent>
      </Card>

      <Card class="surface-card card-interactive">
        <CardHeader class="pb-2"><CardTitle class="text-base">Mixturer</CardTitle></CardHeader>
        <CardContent class="space-y-3">
          <button
            v-for="item in groupedInventory.Mixturer"
            :key="item.id"
            type="button"
            class="flex w-full items-center gap-2 rounded-md border border-border/70 bg-background/60 px-2.5 py-2 text-left"
            @click="openItemDetails(item.id)"
          >
            <component :is="item.icon" class="size-4 text-primary" />
            <span class="flex-1 text-sm">{{ item.name }}</span>
          </button>

          <div class="rounded-md border border-border/70 bg-background/55 p-2.5">
            <p class="mb-2 text-xs font-semibold uppercase tracking-wide text-muted-foreground">Schemalagda mixturer</p>
            <div class="flex items-center justify-between rounded-md border border-border/60 bg-card/70 px-2 py-1.5 text-xs">
              <span class="inline-flex items-center gap-1.5">
                <FlaskConical class="size-3.5 text-primary" />
                Silverdryck
              </span>
              <span class="inline-flex items-center gap-1 text-muted-foreground">
                <Clock3 class="size-3.5" />
                Imorgon 20.20
              </span>
            </div>
          </div>
        </CardContent>
      </Card>
    </div>

    <div v-if="selectedItem" class="fixed inset-0 z-[80] flex items-center justify-center bg-black/60 p-4 backdrop-blur-[2px]" @click.self="closeItemDetails">
      <div class="max-h-[92vh] w-full max-w-6xl overflow-hidden rounded-xl border border-border bg-card shadow-[0_30px_80px_rgba(0,0,0,0.55)]">
        <div class="flex items-center justify-between border-b border-border/80 bg-gradient-to-r from-amber-200/55 via-amber-100/35 to-amber-200/55 px-4 py-2.5 dark:from-amber-800/35 dark:via-amber-700/25 dark:to-amber-800/35">
          <h3 class="text-lg font-semibold tracking-wide" style="font-family: var(--font-display);">{{ selectedItem.name }}</h3>
          <button class="rounded-md p-1 text-muted-foreground transition hover:bg-background/70 hover:text-foreground" @click="closeItemDetails">
            <X class="size-4" />
          </button>
        </div>

        <div class="max-h-[calc(92vh-52px)] overflow-y-auto p-4">
          <div class="grid gap-5 lg:grid-cols-[1.1fr_0.9fr]">
            <section>
              <h4 class="mb-2 text-sm font-semibold uppercase tracking-wide text-muted-foreground">Beskrivning</h4>
              <p class="rounded-md border border-border/70 bg-background/45 p-3 text-sm leading-relaxed">{{ selectedItem.description }}</p>
            </section>

            <section class="rounded-lg border border-border/70 bg-background/35 p-3">
              <div class="mx-auto flex h-64 w-full max-w-xs items-center justify-center rounded-md border border-dashed border-border/70 bg-background/60">
                <img v-if="selectedItem.image" :src="selectedItem.image" :alt="selectedItem.name" class="max-h-56 max-w-full rounded object-contain">
                <div v-else class="flex flex-col items-center gap-2 text-muted-foreground">
                  <component :is="selectedItem.icon" class="size-10" />
                  <span class="text-xs">Ingen bild tillganglig</span>
                </div>
              </div>
            </section>
          </div>

          <div class="mt-5 grid gap-5 lg:grid-cols-2">
            <section class="space-y-4">
              <div class="rounded-lg border border-border/70 bg-background/40">
                <h4 class="border-b border-border/70 px-3 py-2 text-sm font-semibold uppercase tracking-wide">Basfakta</h4>
                <div class="divide-y divide-border/60">
                  <div v-for="fact in selectedItem.facts" :key="`fact-${fact.label}`" class="grid grid-cols-[160px_1fr] gap-3 px-3 py-2 text-sm">
                    <span class="text-muted-foreground">{{ fact.label }}</span>
                    <span class="font-medium">{{ fact.value }}</span>
                  </div>
                </div>
              </div>

              <div class="rounded-lg border border-border/70 bg-background/40">
                <h4 class="border-b border-border/70 px-3 py-2 text-sm font-semibold uppercase tracking-wide">Besvarjelser</h4>
                <div class="divide-y divide-border/60">
                  <div v-for="fact in selectedItem.spellFacts" :key="`spell-${fact.label}`" class="grid grid-cols-[160px_1fr] gap-3 px-3 py-2 text-sm">
                    <span class="text-muted-foreground">{{ fact.label }}</span>
                    <span class="font-medium">{{ fact.value }}</span>
                  </div>
                </div>
              </div>
            </section>

            <section class="space-y-4">
              <div class="rounded-lg border border-border/70 bg-background/40 px-3 py-2">
                <h4 class="text-sm font-semibold uppercase tracking-wide">Anhangare Juomnos nuvarande</h4>
                <p class="mt-2 text-sm">{{ selectedItemComparison }}</p>
              </div>

              <div class="rounded-lg border border-border/70 bg-background/40 p-3">
                <h4 class="mb-2 text-sm font-semibold uppercase tracking-wide">Krav och rekommendationer</h4>
                <div class="space-y-2">
                  <div v-for="req in selectedItem.requirements" :key="req.title" class="rounded-md border border-border/60 bg-card/60 p-2.5 text-sm">
                    <p class="inline-flex items-center gap-2 font-medium">
                      <Check v-if="req.ok" class="size-4 text-emerald-500" />
                      <Info v-else class="size-4 text-amber-500" />
                      {{ req.title }}
                    </p>
                    <p class="mt-1 text-xs text-muted-foreground">{{ req.detail }}</p>
                    <p v-if="req.note" class="mt-1 text-xs text-muted-foreground/90">{{ req.note }}</p>
                  </div>
                </div>
              </div>

              <div class="rounded-lg border border-border/70 bg-background/40 p-3">
                <h4 class="mb-2 text-sm font-semibold uppercase tracking-wide">Modifikationer</h4>
                <ul class="space-y-1 text-sm">
                  <li v-for="mod in selectedItem.modifications" :key="mod" class="inline-flex w-full items-center gap-2 rounded-md border border-border/60 bg-card/60 px-2.5 py-1.5">
                    <Sparkles class="size-3.5 text-primary" />
                    <span>{{ mod }}</span>
                  </li>
                </ul>
              </div>
            </section>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
