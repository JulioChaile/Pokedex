<template>
  <section class="column acerca_de_container items-center">
    <div class="logo oculto mostrar active q-ma-md q-mt-xl" @click="logo"></div>
    <div class="column items-center info">
      <div class="marca text-h3 text-weight-bold q-pa-xs">
        PaTSUM
      </div>
      <div class="slogan text-center text-subtitle2 text-weight-medium">
        <span>
          Aplicaciones sencillas
        </span>
        <br>
        <span>
          Para problemas cotidianos
        </span>
      </div>
      <div class="sitio_web text-caption q-mt-xl">
        Sitio web: <a id="sitio_web" href="https://juliochaile.github.io/PaTSUM">PaTSUM</a>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data () {
    return {
      loading: true
    }
  },
  mounted () {
    const logo = document.getElementsByClassName('logo')[0]
    const slogan = document.getElementsByClassName('slogan')[0]
    const marca = document.getElementsByClassName('marca')[0]
    const sitioWeb = document.getElementsByClassName('sitio_web')[0]
    const a = document.getElementById('sitio_web')

    setTimeout(() => {
      logo.classList.toggle('oculto')
    }, 10)

    setTimeout(() => {
      logo.classList.toggle('mostrar')
      logo.classList.toggle('active')
      this.loading = false
    }, 2000)

    setTimeout(() => {
      slogan.classList.toggle('mostrar')
      marca.classList.toggle('mostrar')
      sitioWeb.classList.toggle('mostrar')
      a.classList.toggle('mostrar')
    }, 2500)
  },
  methods: {
    logo () {
      if (this.loading) { return }

      const logo = document.getElementsByClassName('logo')[0]

      logo.classList.toggle('active')
    }
  }
}
</script>

<style scoped>
  .acerca_de_container {
    max-width: none;
  }

  .info {
    margin-top: 250px;
    color: transparent;
  }
  a {
    color: transparent;
    transition: 1s ease 1.5s;
  }

  .marca {
    transition: 1s;
  }
  .slogan {
    transition: 1s ease 0.75s;
  }
  .sitio_web {
    transition: 1s ease 1.5s;
  }
  .marca.mostrar {
    color: black;
  }
  .slogan.mostrar {
    color: #9e9e9e !important;
  }
  .sitio_web.mostrar {
    color: black;
  }
  a.mostrar {
    color: initial;
  }

  .logo {
    top: 0;
    width: 200px;
    height: 200px;
    position: absolute;
    border-radius: 50%;
    border: solid 25px;
    border-bottom-color: rgb(35, 52, 199);
    border-left-color: transparent;
    border-right-color: transparent;
    border-top-color: transparent;
  }

  .logo::before {
    content: '';
    width: calc(50% + 25px);
    height: calc(50% + 25px);
    position: absolute;
    top: -25px;
    left: -25px;
    border-top-left-radius: 100%;
    border-left: solid 25px;
    border-top: solid 25px;
    border-left-color: rgb(35, 52, 199);
    border-top-color: rgb(35, 52, 199);
    transform: rotate(0deg);
    transition: top 0.8s,
                left 0.8s,
                transform 0.8s,
                width 0.8s,
                height 0.8s;
  }
  .logo::after {
    content: '';
    width: calc(50% + 25px);
    height: calc(50% + 25px);
    position: absolute;
    top: 25px;
    right: -25px;
    border-radius: 50%;
    border: solid 25px;
    border-bottom-color: rgb(35, 52, 199);
    border-left-color: rgb(35, 52, 199);
    border-right-color: transparent;
    border-top-color: transparent;
    transform: rotate(135deg);
    transition: top 0.8s,
                right 0.8s,
                transform 0.8s;
  }

  .logo.active::before {
    content: '';
    width: calc(40% + 25px);
    height: calc(40% + 25px);
    top: 75px;
    left: 32px;
    transform: rotate(-135deg);
  }
  .logo.active::after {
    content: '';
    top: 10px;
    right: 25px;
    transform: rotate(-45deg);
  }
  .logo.oculto {
    top: -322px;
  }
  .logo.oculto::before {
    top: calc(-322px + 75px);
  }
  .logo.oculto::after {
    top: calc(-322px + 10px);
  }
  .logo.mostrar.active {
    transition: top 1s
  }
  .logo.mostrar.active::after {
    transition: top 1s ease 1s;
  }
  .logo.mostrar.active::before {
    transition: top 1s ease 0.5s;
  }
</style>
