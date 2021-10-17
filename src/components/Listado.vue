<template>
  <div class="full-width flex justify-center">
    <!-- Item pokemon de la lista -->
    <div
      v-for="p in Pokemons"
      :key="p.id"
      :id="p.id + 'pokemon'"
      class="pokemon-item relative-position cursor-pointer"
      @click="selectPokemon(p)"
    >
      <!-- Id -->
      <div class="absolute-top-left text-overline q-ml-sm text-weight-bolder">
        N° {{ p.id }}
      </div>

      <div class="row">
        <!-- Imagen -->
        <div class="img-pokemon flex items-center justify-center">
          <img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${p.id}.png`"
            :alt="`${p.name}`"
          >
        </div>

        <q-separator
          vertical
          spaced
          inset
          size="2px"
        />

        <div class="col column" style="max-height: 80px">
          <!-- Nombre y peso -->
          <div class="col-7 row q-py-sm q-pr-sm">
            <div class="col-8 text-left text-overline text-weight-medium text-capitalize" style="font-size: 1.25rem">
              {{ p.name }}
            </div>
            <div class="col-4 text-right text-caption text-weight-bold">
              {{ p.weight }} kg
            </div>
          </div>

          <!-- Tipos del pokemon -->
          <div>
            <q-badge
              v-for="t in p.tiposEn"
              :key="t"
              squared
              class="text-uppercase q-mr-xs"
              :label="TiposEs[t] ? TiposEs[t].name : t"
              :style="`background: ${TiposEs[t] ? TiposEs[t].color : ''}`"
            />
          </div>
        </div>
      </div>

      <q-separator size="2px" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Listado',
  props: ['Pokemons', 'TiposEs'],
  methods: {
    selectPokemon (p) {
      this.$emit('selectPokemon', p)
    }
  }
}
</script>

<style lang="scss">
  .pokemon-item {
    width: 100%;
    height: 80px;
  }

  .img-pokemon {
    width: 80px;
    height: 80px;
    overflow: hidden;
  }
</style>
