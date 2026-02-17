<script setup lang="ts">
import { Sun, Moon, Mail, Swords, Settings, Shield, Users } from 'lucide-vue-next'

const colorMode = useColorMode()

function toggleTheme() {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
}
</script>

<template>
  <header class="sticky top-0 z-50 header-decorative-edge bg-gradient-to-r from-card via-card to-card/90 dark:from-card dark:via-card dark:to-card/90 backdrop-blur-sm">
    <div class="flex h-14 items-center justify-between px-4">
      <!-- Logo & Season -->
      <div class="flex items-center gap-4">
        <NuxtLink to="/" class="flex items-center gap-2 group">
          <Shield class="size-6 text-primary transition-transform group-hover:scale-110" />
          <span
            class="text-xl font-bold tracking-[0.25em] text-primary uppercase"
            style="font-family: var(--font-display-decorative);"
          >
            Lanista
          </span>
        </NuxtLink>
        <Separator orientation="vertical" class="hidden h-6 sm:block" />
        <div class="hidden text-xs sm:flex sm:items-center sm:gap-3">
          <span class="font-medium text-foreground/80" style="font-family: var(--font-display); font-size: 0.7rem;">
            Säsong BETA Special III
          </span>
          <span class="flex items-center gap-1 text-muted-foreground">
            <Users class="size-3" />
            <span class="tabular-nums">233</span> online
          </span>
        </div>
      </div>

      <!-- Actions -->
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
        </Button>
        <Button variant="ghost" size="icon" title="Strider" class="hover:bg-accent/30 transition-all hover:scale-105">
          <Swords class="size-4" />
        </Button>
        <Button variant="ghost" size="icon" title="Inställningar" class="hover:bg-accent/30 transition-all hover:scale-105">
          <Settings class="size-4" />
        </Button>
      </div>
    </div>

    <!-- Ticker Bar -->
    <div class="border-t border-border/50 bg-muted/30 px-4 py-1 overflow-hidden">
      <div class="animate-marquee whitespace-nowrap text-xs text-muted-foreground">
        <span class="mx-4 font-bold text-primary uppercase tracking-wider" style="font-size: 0.65rem;">Topp 20 EP idag:</span>
        <span class="mx-4">EP - grad 45+2 salamanth</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">16. Anhängare Reana Ruff - <strong class="text-foreground/80">10 803 EP</strong> - grad 43 dvärg</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">17. Anhängare Larre - <strong class="text-foreground/80">10 795 EP</strong> - grad 43 dvärg</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">18. VäktarDåren Cowen II - <strong class="text-foreground/80">10 698 EP</strong> - grad 34 dvärg</span>
        <span class="mx-2 text-border">|</span>
        <span class="mx-4">19. Anhängare Duppen - <strong class="text-foreground/80">10 500 EP</strong> - grad 41 människa</span>
      </div>
    </div>
  </header>
</template>
