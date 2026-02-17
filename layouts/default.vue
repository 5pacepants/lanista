<script setup lang="ts">
import { Menu } from 'lucide-vue-next'

const mobileMenuOpen = ref(false)
</script>

<template>
  <div class="min-h-screen bg-background">
    <!-- Header -->
    <GameTheHeader />

    <div class="flex">
      <!-- Mobile menu trigger -->
      <div class="fixed bottom-4 left-4 z-50 lg:hidden">
        <Button size="icon" class="rounded-full shadow-lg" @click="mobileMenuOpen = true">
          <Menu class="size-5" />
        </Button>
      </div>

      <!-- Mobile Navigation Sheet -->
      <Sheet v-model:open="mobileMenuOpen">
        <SheetContent side="left" class="w-72 overflow-y-auto p-0">
          <SheetHeader class="p-4 pb-0">
            <SheetTitle class="text-primary">Navigation</SheetTitle>
          </SheetHeader>
          <GameNavigationSidebar />
        </SheetContent>
      </Sheet>

      <!-- Desktop Left Sidebar -->
      <aside class="hidden w-64 shrink-0 border-r bg-sidebar overflow-y-auto lg:block" style="height: calc(100vh - 4.5rem); position: sticky; top: 4.5rem;">
        <GameNavigationSidebar />
      </aside>

      <!-- Main Content -->
      <main class="flex-1 min-w-0 px-4 py-5 lg:px-8">
        <slot />
      </main>

      <!-- Desktop Right Sidebar -->
      <aside class="hidden w-72 shrink-0 border-l bg-sidebar overflow-y-auto xl:block" style="height: calc(100vh - 4.5rem); position: sticky; top: 4.5rem;">
        <GameGladiatorPanel />
      </aside>
    </div>
  </div>
</template>
