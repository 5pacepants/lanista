<script setup lang="ts">
import { Sun, Moon, Mail, Swords, Settings, Users, SlidersHorizontal } from 'lucide-vue-next'
import logoSrc from '~/assets/logo.png'

const colorMode = useColorMode()
const settingsOpen = ref(false)

const receiveItemsFrom = ref('Alla')
const hideMaxDamage = ref(true)
const spoilerFreeBattles = ref(false)
const expandedRoundInfo = ref(true)
const battleSoundNotifications = ref(true)
const allowAffixNameChanges = ref(false)
const hideRollingToplist = ref(false)
const hideNewInterface = ref(false)
const deleteGladiator = ref(false)

function toggleTheme() {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
}
</script>

<template>
  <header class="sticky top-0 z-50 header-decorative-edge bg-gradient-to-r from-card/95 via-card to-card/90 backdrop-blur-sm">
    <div class="flex h-14 items-center justify-between px-4">
      <div class="flex items-center gap-4">
        <NuxtLink to="/" class="group">
          <img
            :src="logoSrc"
            alt="Lanista"
            class="h-8 transition-transform group-hover:scale-105 dark:brightness-125 dark:contrast-90"
          />
        </NuxtLink>
        <Separator orientation="vertical" class="hidden h-6 sm:block" />
        <div class="hidden text-xs sm:flex sm:items-center sm:gap-3">
          <span class="font-semibold text-foreground/85" style="font-family: var(--font-display); font-size: 0.72rem;">
            Säsong BETA Special III
          </span>
          <span class="flex items-center gap-1 rounded-full border border-primary/20 bg-primary/10 px-2 py-0.5 text-primary">
            <Users class="size-3" />
            <span class="tabular-nums font-semibold">233</span> online
          </span>
        </div>
      </div>

      <div class="flex items-center gap-1">
        <ClientOnly>
          <Button
            variant="ghost"
            size="icon"
            @click="toggleTheme"
            :title="colorMode.value === 'dark' ? 'Ljust tema' : 'Mörkt tema'"
            class="hover:bg-accent/30 transition-all hover:scale-105"
          >
            <Sun v-if="colorMode.value === 'dark'" class="size-4" />
            <Moon v-else class="size-4" />
          </Button>
          <template #fallback>
            <Button variant="ghost" size="icon" class="hover:bg-accent/30">
              <Moon class="size-4" />
            </Button>
          </template>
        </ClientOnly>
        <Button variant="ghost" size="icon" title="Meddelanden" class="hover:bg-accent/30 transition-all hover:scale-105 relative">
          <Mail class="size-4" />
          <span class="absolute right-1.5 top-1.5 size-2 rounded-full bg-primary badge-notify" />
        </Button>
        <Button variant="ghost" size="icon" title="Strider" class="hover:bg-accent/30 transition-all hover:scale-105">
          <Swords class="size-4" />
        </Button>
        <Button variant="ghost" size="icon" title="Inställningar" class="hover:bg-accent/30 transition-all hover:scale-105" @click="settingsOpen = true">
          <Settings class="size-4" />
        </Button>
      </div>
    </div>

    <div class="border-t border-border/50 bg-muted/35 px-4 py-1 overflow-hidden">
      <div class="animate-marquee whitespace-nowrap text-xs text-muted-foreground">
        <span class="mx-4 font-bold text-primary uppercase tracking-wider section-kicker">Topp 20 EP idag:</span>
        <span class="mx-4">EP - grad 45+2 salamanth</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">16. Anhängare Reana Ruff - <strong class="text-foreground/90">10 803 EP</strong> - grad 43 dvärg</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">17. Anhängare Larre - <strong class="text-foreground/90">10 795 EP</strong> - grad 43 dvärg</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">18. VäktarDåren Cowen II - <strong class="text-foreground/90">10 698 EP</strong> - grad 34 dvärg</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">19. Anhängare Duppen - <strong class="text-foreground/90">10 500 EP</strong> - grad 41 människa</span>
      </div>
    </div>
  </header>

  <Sheet v-model:open="settingsOpen">
    <SheetContent side="right" class="w-[92vw] max-w-md overflow-y-auto">
      <SheetHeader>
        <SheetTitle class="flex items-center gap-2" style="font-family: var(--font-display);">
          <SlidersHorizontal class="size-4 text-primary" />
          Övriga inställningar
        </SheetTitle>
      </SheetHeader>

      <div class="mt-5 space-y-4">
        <div class="space-y-2">
          <label class="section-kicker">Tillåt ta emot föremål från</label>
          <Select v-model="receiveItemsFrom">
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

        <div class="space-y-3 pt-1">
          <div class="flex items-center gap-2">
            <Checkbox id="hide-max-damage" v-model:checked="hideMaxDamage" />
            <label for="hide-max-damage" class="text-sm cursor-pointer">Dölj maxskada i statistik och ärans sal?</label>
          </div>
          <div class="flex items-center gap-2">
            <Checkbox id="spoiler-free" v-model:checked="spoilerFreeBattles" />
            <label for="spoiler-free" class="text-sm cursor-pointer">Spoilerfria strider</label>
          </div>
          <div class="flex items-center gap-2">
            <Checkbox id="expanded-round-info" v-model:checked="expandedRoundInfo" />
            <label for="expanded-round-info" class="text-sm cursor-pointer">Utökad information varje runda</label>
          </div>
          <div class="flex items-center gap-2">
            <Checkbox id="battle-sound-notifications" v-model:checked="battleSoundNotifications" />
            <label for="battle-sound-notifications" class="text-sm cursor-pointer">Tillåt ljudnotifikationer för strider</label>
          </div>
          <div class="flex items-center gap-2">
            <Checkbox id="allow-affix-name-changes" v-model:checked="allowAffixNameChanges" />
            <label for="allow-affix-name-changes" class="text-sm cursor-pointer">Tillåt namnändringar (affix) från klan</label>
          </div>
          <div class="flex items-center gap-2">
            <Checkbox id="hide-rolling-toplist" v-model:checked="hideRollingToplist" />
            <label for="hide-rolling-toplist" class="text-sm cursor-pointer">Dölj rullande topplista</label>
          </div>
          <div class="flex items-center gap-2">
            <Checkbox id="hide-new-interface" v-model:checked="hideNewInterface" />
            <label for="hide-new-interface" class="text-sm cursor-pointer">Dölj nytt gränssnitt</label>
          </div>
          <div class="flex items-center gap-2 pt-2 border-t border-border/60">
            <Checkbox id="delete-gladiator" v-model:checked="deleteGladiator" />
            <label for="delete-gladiator" class="text-sm cursor-pointer text-destructive font-medium">Radera gladiator</label>
          </div>
        </div>
      </div>
    </SheetContent>
  </Sheet>
</template>
