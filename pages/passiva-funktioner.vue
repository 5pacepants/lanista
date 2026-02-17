<script setup lang="ts">
const passiveEnabled = ref(true)
const passiveKnega = ref(true)
const passiveCombatTraining = ref(false)
const passiveWorkshop = ref(false)
const passiveBuildings = ref(false)
const passiveBeasts = ref(false)
const passiveCityBuildings = ref(false)
</script>

<template>
  <div class="space-y-5">
    <section class="hero-shell rounded-xl p-4 lg:p-5">
      <h1 class="text-3xl font-bold tracking-tight lg:text-4xl" style="font-family: var(--font-display);">
        Passiva funktioner
      </h1>
    </section>

    <Card class="surface-card card-interactive">
      <CardContent class="space-y-4 pt-5">
        <p class="text-sm leading-relaxed text-foreground/90">
          Din gladiator börjar utföra de passiva uppgifter du har ställt in att den ska göra en gång per halvtimme när du har varit
          inaktiv i minst 30 minuter. Din gladiator markeras automatiskt som aktiv så fort du gör något med din gladiator, alltifrån att
          markera utmaningar som lästa till att utföra en strid. Du behöver alltså ha varit inaktiv i 30 minuter för att en passiv funktion
          ska utföras (sker varje hel- och halvtimme).
        </p>

        <p class="text-sm leading-relaxed text-foreground/90">
          Undantaget är strider där du inte själv genomför något aktivt, t.ex om någon antar en tidigare skickad utmaning, en turneringsstrid
          utspelas som du tidigare anmält dig till osv.
        </p>

        <p class="text-sm leading-relaxed text-foreground/90">
          Nedan kan du välja vad för typ av passiv uppgift du vill att din gladiator ska genomföra. Din gladiator använder sin tillgängliga tid
          en gång varje halvtimme för att utföra den passiva uppgiften. Tid (rundor) som används till passiva uppgifter är en en tredjedel så
          effektiva som om du hade utfört samma uppgift aktivt. Om du t.ex knegar 125 rundor passivt så räknas det enbart som cirka 42 rundor.
        </p>

        <p class="text-sm leading-relaxed text-foreground/90">
          Observera att din gladiator enbart börjar utföra passiva funktioner om den fortfarande är markerad som passiv, är grad 4 eller högre,
          har minst 10 rundor tillgängliga, inte har haft någon aktivitet den senaste halvtimmen och inte är i sjukstugan eller fängelset.
          Passiva funktioner utförs varje hel- och halvtimme.
        </p>

        <Separator />

        <div class="space-y-3">
          <div class="flex items-center gap-2">
            <Checkbox id="passive-enabled" v-model:checked="passiveEnabled" />
            <label for="passive-enabled" class="text-sm cursor-pointer font-medium">Passiva funktioner aktiverade</label>
          </div>
          <p class="text-xs text-muted-foreground">(Klicka ur för att inaktivera passiva funktioner helt)</p>
        </div>

        <Separator />

        <div class="space-y-4">
          <div class="space-y-1">
            <div class="flex items-center gap-2">
              <Checkbox id="passive-knega" v-model:checked="passiveKnega" />
              <label for="passive-knega" class="text-sm cursor-pointer font-medium">Knega</label>
            </div>
            <p class="text-sm text-muted-foreground leading-relaxed">
              Genom att ställa in den passiva funktionen "Knega" så kommer din gladiator att bege sig till arbetsdistriktet och knega för den
              tillgängliga tiden för lite extra silvermynt.
            </p>
          </div>

          <div class="space-y-1">
            <div class="flex items-center gap-2">
              <Checkbox id="passive-combat-training" v-model:checked="passiveCombatTraining" />
              <label for="passive-combat-training" class="text-sm cursor-pointer font-medium">Stridsträning</label>
            </div>
            <p class="text-sm text-muted-foreground leading-relaxed">
              Med stridsträning kan din gladiator få erfarenhetspoäng när du som lanista inte finns där för att aktivt styra. Din gladiator får
              en fast summa EP per halvtimme baserat på din nuvarande grad. När du är aktiv kan du fortfarande använda träningslokalen i
              stadsbyggnader eller klan/sammanslutningsbyggnader.
            </p>
          </div>

          <div class="space-y-1">
            <div class="flex items-center gap-2">
              <Checkbox id="passive-workshop" v-model:checked="passiveWorkshop" />
              <label for="passive-workshop" class="text-sm cursor-pointer font-medium">Verkstaden</label>
            </div>
            <p class="text-sm text-muted-foreground leading-relaxed">
              Din gladiator kan under sin passiva tid smida på arbeten i verkstaden. Väljer du det här alternativet behöver du välja ett smide
              att passivt arbeta på. Om smidet blir klart under den passiva tiden så börjar din gladiator istället knega i arbetsdistriktet.
            </p>
          </div>

          <div class="space-y-1">
            <div class="flex items-center gap-2">
              <Checkbox id="passive-buildings" v-model:checked="passiveBuildings" />
              <label for="passive-buildings" class="text-sm cursor-pointer font-medium">Byggnader för klaner och sammanslutningar</label>
            </div>
            <p class="text-sm text-muted-foreground leading-relaxed">
              För att göra klanen eller sammanslutningen nöjd så kan din gladiator välja att spendera rundor på byggnader under sin passiva tid.
              Du väljer vilken byggnad som rundorna skall spenderas på nedan efter att det här valet är aktivt. Rundorna blir mindre effektiva ju
              fler medlemmar klanen eller sammanslutningen har. För varje medlem klanen eller sammanslutningen har så minskar effektiviteten av
              rundorna med ytterligare 1%.
            </p>
          </div>

          <div class="space-y-1">
            <div class="flex items-center gap-2">
              <Checkbox id="passive-beasts" v-model:checked="passiveBeasts" />
              <label for="passive-beasts" class="text-sm cursor-pointer font-medium">Odjur</label>
            </div>
            <p class="text-sm text-muted-foreground leading-relaxed">
              Som Lanista kan du välja att skicka iväg din gladiator att strida mot odjur för att tjäna erfarenhetspoäng och kunna plundra odjur.
              Du får hälften av den normala erfarenheten och silvermynt från odjur när strider sker passivt. Odjur väljs ut slumpmässigt mellan
              2 grader lägre än din gladiator och din gladiators grad. Dina standardinställningar för odjur används i striderna och startar
              enbart om din gladiator har minst 90% KP. Din gladiators kvot för dagliga odjur används i dessa passiva strider och om du inte
              längre har några dagliga odjurstrider kvar i din kvot kommer din gladiator börja knega istället.
            </p>
          </div>

          <div class="space-y-1">
            <div class="flex items-center gap-2">
              <Checkbox id="passive-city-buildings" v-model:checked="passiveCityBuildings" />
              <label for="passive-city-buildings" class="text-sm cursor-pointer font-medium">Stadsbyggnader</label>
            </div>
            <p class="text-sm text-muted-foreground leading-relaxed">
              Om du vill allas väl kan du skicka iväg din gladiator att hjälpa till med konstruktionen av stadsbyggnader. Under sin passiva tid så
              spenderar i sådana fall din gladiator sina rundor på en utvald stadsbyggnad.
            </p>
          </div>
        </div>
      </CardContent>
    </Card>
  </div>
</template>
