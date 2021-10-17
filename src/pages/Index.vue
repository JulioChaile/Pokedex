<template>
  <q-page class="flex flex-center border-pokedex-extremo bg-white">
    <!-- Borde izquierdo -->
    <div class="absolute border-pokedex-izquierdo">
    </div>

    <!-- Borde derecho -->
    <div class="absolute border-pokedex-derecho">
    </div>

    <div class="absolute column justify-end text-capitalize text-overline nombre-pokemon">
      <div
        v-if="panel === 'pokemon'"
      >
        {{ PokemonSeleccionado.name }}
      </div>
      <q-input
        v-if="panel === 'listado'"
        v-model="busqueda"
        class="busqueda-form"
        label="Busqueda"
        dense
        type="text"
        color="negative"
      />
    </div>

    <!-- Secciones -->
    <q-tab-panels
      v-model="panel"
      animated
      class="paneles-container full-width q-pa-none"
    >
      <q-tab-panel name="listado">
        <Listado
          :Pokemons="Pokemons.filter(p => p.name.includes(busqueda.toLowerCase()))"
          :TiposEs="TiposEs"
          @selectPokemon="selectPokemon"
        />
      </q-tab-panel>

      <q-tab-panel name="pokemon">
        <Pokemon
          :Pokemon="PokemonSeleccionado"
          :MovesEs="MovesEs"
          :TiposEs="TiposEs"
          @atras="panel = 'listado'"
        />
      </q-tab-panel>

      <q-tab-panel name="acercaDe">
        <div
          class="absolute-top-left cursor-pointer text-overline flex items-center"
          @click="$emit('force')"
        >
          <q-icon
            name="navigate_before"
            size="md"
          />
          <span style="font-size: 0.85rem">Atras</span>
        </div>
        <AcercaDe />
      </q-tab-panel>
    </q-tab-panels>
  </q-page>
</template>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'
import Listado from '../components/Listado'
import Pokemon from '../components/Pokemon'
import AcercaDe from '../components/AcercaDe'

const api = axios.create({ baseURL: 'https://pokeapi.co/api/v2' })

// Colores oficiales segun el tipo de pokemon extraidos de https://veekun.com/dex/media/types/en/
const typesColor = {
  bug: '#a8b820',
  dark: '#705848',
  dragon: '#7038f8',
  electric: '#f8d030',
  fairy: '#f8a0e0',
  fighting: '#903028',
  fire: '#f05030',
  flying: '#a890f0',
  ghost: '#705898',
  grass: '#78c850',
  ground: '#e0c068',
  ice: '#98d8d8',
  normal: '#a8a878',
  poison: '#a040a0',
  psychic: '#f85888',
  rock: '#b8a038',
  shadow: '#403246',
  steel: '#b8b8d0',
  unknown: '#68a090',
  water: '#6890f0'
}

export default defineComponent({
  name: 'PageIndex',
  props: ['acercaDe'],
  components: {
    Listado,
    Pokemon,
    AcercaDe
  },
  data () {
    return {
      panel: 'listado',
      busqueda: '',
      Pokemons: [],
      TiposEs: {},
      MovesEs: {},
      PokemonSeleccionado: {}
    }
  },
  mounted () {
    if (this.$route.query.panel === 'acercaDe') {
      this.panel = this.$route.query.panel
      return
    }

    // Pido los tipos de pokemon antes porque la api devuelve los datos del pokemon en ingles
    // De esta forma consigo un listado en español y no hay problemas con el renderizado luego
    api.get('/type/').then(r => {
      const types = r.data.results.map(t => t.name)

      types.forEach((t, i) => {
        this.getTipoEs(t).then(r => {
          this.TiposEs[t] = {
            name: r,
            color: typesColor[t]
          }
        })
      })
    })

    api.get('/move/?limit=844').then(r => {
      const moves = r.data.results.map(t => t.name)

      moves.forEach((m, i) => {
        this.getMoveEs(m).then(r => {
          this.MovesEs[m] = r
        })
      })
    })

    // Pido a la api el listado de los 151 pokemons de la primera generacion
    api.get('/pokemon/?limit=151')
      .then(r => {
        r.data.results.forEach((p, i) => {
          this.Pokemons.push({
            name: p.name,
            id: p.id
          })
          this.getPokemon(p.name).then(r => {
            this.Pokemons[i] = r
          })
        })
      })
  },
  watch: {
    panel () {
      if (this.panel === 'listado') {
        this.$nextTick(() => {
          if (document.getElementById(this.PokemonSeleccionado.id + 'pokemon')) {
            document.getElementById(this.PokemonSeleccionado.id + 'pokemon').scrollIntoView(true)
          }
        })
      }
    }
  },
  methods: {
    /*
    Pide a la api los datos de un pokemon

    n: Nombre del pokemon

    Retorna
    pokemon: Objecto
    {
      weigth: Peso del pokemon,
      tiposEs: Tipos del pokemon,
      name: Nombre del pokemon
    }
    */
    async getPokemon (n) {
      let pokemon

      await api.get(`/pokemon/${n}`)
        .then(r => {
          // Convierto el peso de hectometros a kilogramos
          const weight = parseFloat(r.data.weight) / 10
          const tiposEn = r.data.types.map(t => t.type.name)
          const movesEn = r.data.moves.map(m => m.move.name)
          const name = n
          const id = r.data.id

          pokemon = {
            weight,
            tiposEn,
            movesEn,
            name,
            id
          }
        })

      return pokemon
    },
    /*
    Toma el nombre de un tipo de pokemon en ingles y lo devuelve en español

    t: Nombre del tipo de pokemon

    Retorna
    tipo: Objeto
    {
      name: Tipo de pokemon en español
    }
    */
    async getTipoEs (t) {
      let tipo

      await api.get(`/type/${t}`)
        .then(r => {
          const names = r.data.names

          // Si no lo encuentra en español, lo uso en ingles
          const i = names.findIndex(n => n.language.name === 'es')
          const j = names.findIndex(n => n.language.name === 'en')

          tipo = i >= 0
            ? names[i].name
            : names[j].name
        })

      return tipo
    },
    async getMoveEs (m) {
      let move

      await api.get(`/move/${m}`)
        .then(r => {
          const names = r.data.names

          // Si no lo encuentra en español, lo uso en ingles
          const i = names.findIndex(n => n.language.name === 'es')
          const j = names.findIndex(n => n.language.name === 'en')

          move = i >= 0
            ? names[i].name
            : names[j].name
        })

      return move
    },
    selectPokemon (p) {
      this.PokemonSeleccionado = p

      api.get(`/pokemon-species/${p.id}`)
        .then(r => {
          const descripciones = r.data.flavor_text_entries

          const i = descripciones.findIndex(n => n.language.name === 'es')
          const j = descripciones.findIndex(n => n.language.name === 'en')

          this.PokemonSeleccionado.descripcion = i >= 0
            ? descripciones[i].flavor_text
            : descripciones[j].flavor_text

          this.panel = 'pokemon'
        }).catch(e => {
          this.PokemonSeleccionado.descripcion = ''
          this.panel = 'pokemon'
        })
    }
  }
})
</script>

<style lang="scss">
  .border-pokedex-extremo::before, .border-pokedex-extremo::after {
    content: '';
    position: absolute;
    width: 30px;
    height: 30px;
    border: 5px solid black;
    border-radius: 5px;
    background: $negative;
  }

  .border-pokedex-extremo::before {
    bottom: -1px;
    right: 0;
  }

  .border-pokedex-extremo::after {
    top: -45px;
    right: 0;
    z-index: 2000;
  }

  .border-pokedex-derecho {
    right:0;
    bottom: 28px;
    z-index: 2000;
    width: 30px;
    height: calc(100% - 10px);
    background: $negative;
    border-radius: 5px;
    border: 5px solid black;
  }

  .border-pokedex-izquierdo {
    left:0;
    width: 30px;
    height: 100%;
    background: $negative;
    border-right: 5px solid black;
    border-left: 5px solid black;
  }

  .q-page {
    padding-left: 30px;
    padding-right: 30px;
  }

  .paneles-container {
    // Resto a la altura de la pantalla las dimensiones del header y del footer para poder hacer scroll interno
    height: calc(100vh - 130px - 50px);
    overflow: hidden;
  }

  .q-tab-panel {
    padding: 0;
  }

  .nombre-pokemon {
    right: 30px;
    z-index: 3000;
    width: calc(100% - 200px);
    height: 45px;
    top: -45px;
    font-size: 1.25rem;
  }
</style>
