<template>
  <div class="q-pt-sm relative-position">
    <div
      class="absolute-top-left cursor-pointer text-overline flex items-center"
      @click="$emit('atras')"
    >
      <q-icon
        name="navigate_before"
        size="md"
        :style="`color: ${TiposEs[Pokemon.tiposEn[0]].color} !important`"
      />
      <span style="font-size: 0.85rem">Atras</span>
    </div>

    <div class="full-width flex justify-center">
      <img
        :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${Pokemon.id}.png`"
        :alt="Pokemon.name"
        width="150"
        height="150"
      >
    </div>

    <div class="column items-start text-overline text-overline q-pl-md">
      <div>
        <span class="text-weight-bolder">PESO:</span> {{ Pokemon.weight }} kg
      </div>
      <div>
        <span class="text-weight-bolder">TIPO:</span>
        <q-badge
          v-for="t in Pokemon.tiposEn"
          :key="t"
          squared
          class="text-uppercase q-mr-xs"
          :label="TiposEs[t] ? TiposEs[t].name : t"
          :style="`background: ${TiposEs[t] ? TiposEs[t].color : ''}`"
        />
      </div>
      <div>
        <span class="text-weight-bolder">DESCRIPCIÓN:</span> {{ Pokemon.descripcion }}
      </div>
      <span class="text-weight-bolder">MOVIMIENTOS:</span>
      <div class="full-width row">
        <div
          v-for="m in Pokemon.movesEn"
          :key="m"
          class="col-6 items-center text-left"
        >
          {{ MovesEs[m] || m }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Pokemon',
  props: ['Pokemon', 'MovesEs', 'TiposEs']
}
</script>
