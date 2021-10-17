<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="header-pokedex bg-negative" elevated>
      <div class="container-big-light flex justify-center items-center absolute">
        <div
          class="big-light-boarder cursor-pointer"
          @click="acercaDe()"
        >
          <div class="big-light blue">
            <div class="big-dot light-blue"></div>
          </div>
        </div>
      </div>

      <div class="container-luces row items-center justify-center">
        <q-avatar size="sm" color="red" />
        <q-avatar size="sm" color="yellow" class="q-mx-sm" />
        <q-avatar size="sm" color="green" />
      </div>
    </q-header>

    <q-footer class="bg-negative footer-pokedex" elevated>
    </q-footer>

    <q-page-container>
      <router-view
        :key="force"
        @force="acercaDe('listado')"
      />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MainLayout',
  data () {
    return {
      force: 0
    }
  },
  methods: {
    acercaDe (p = 'acercaDe') {
      this.$router.push({
        query: {
          panel: p
        }
      })

      setTimeout(() => {
        this.force++
      }, 500)
    }
  }
})
</script>

<style lang="scss">
.header-pokedex {
  height: 130px;
  border: 5px solid black;
  border-top-left-radius: 25px;
  border-top-right-radius: 25px;
  width: calc(100vh * (9 / 16));
  max-width: 100vw;
  min-width: 360px;
  margin: 0 auto;
}

.header-pokedex::before {
  content: '';
  position: absolute;
  width: calc(100% - 120px);
  height: 50px;
  right: -5px;
  bottom: -6px;
  background: white;
  z-index: 1000;
  border: none;
  border-top: 5px solid black;
}

.header-pokedex::after {
  content: '';
  position: absolute;
  height: 50px;
  width: 70px;
  left: 100px;
  bottom: 12px;
  z-index: 2000;
  background-color: $negative;
  border-bottom: 6px solid black;
  transform: rotate(-40deg);
}

.footer-pokedex {
  height: 50px;
  border: 5px solid black;
  width: calc(100vh * (9 / 16));
  max-width: 100vw;
  min-width: 360px;
  margin: 0 auto;
  border-bottom-left-radius: 25px;
  border-bottom-right-radius: 25px;
  border-top-right-radius: 8px;
}

.big-light-boarder {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 5px;
  height: 90px;
  width: 90px;
  border: solid black;
  border-radius: 50%;
  background-color: white;
}

.big-light {
  height: 75px;
  width: 75px;
  border-radius: 50%;
  border: solid black;
}

.blue {
  background-color: #3298cb;
}

.light-blue {
  background-color: #85bdfe;
}

.big-dot {
  height: 15px;
  width: 15px;
  position: relative;
  top: 14px;
  left: 10px;
  border-radius: 50%;
}

.container-big-light {
  width: 130px;
  height: 130px;
  z-index: 3000;
}

.container-luces {
  width: calc(100% - 130px);
  height: 80px;
  margin-left: 130px;
}
</style>
