<template>
  <div id="app" class="row">
    <div class="container">
        <div class="news">
            <News/>
        </div>
    </div>
  </div>
</template>

<script>
  import News from '~/components/News.vue'

  export default {
    name: 'app',
    layout: 'default',
    components: {
      News,
    },

    head: {
      title: 'La confrérie du mog - Final Fantasy XIV Actualités',
      meta: [
          { charset: 'utf-8' },
          { name: 'viewport', content: 'width=device-width, initial-scale=1' },
          { hid: 'description', name: 'description', content: 'Suivez les dernières actualités de Final Fantasy XIV !' },

          // Open Graph
          { name: 'og:title', content: 'La confrérie du mog - Final Fantasy XIV, Cerberus'},
          { name: 'og:description', content: 'Suivez les dernières actualités de Final Fantasy XIV !' },
          { name: 'og:type', content: 'website' },
          { name: 'og:url', content: 'http://www.laconfreriedumog.fr' },
          { name: 'og:image', content: 'https://wwww.laconfreriedumog.fr/logo.png' },
          // Twitter Card
          { name: 'twitter:card', content: 'summary' },
          { name: 'twitter:site', content: '@Ozzlo_' },
          { name: 'twitter:title', content: 'La confrérie du mog - Final Fantasy XIV, Cerberus' },
          { name: 'twitter:description', content: 'Suivez les dernières actualités de Final Fantasy XIV !' },
          { name: 'twitter:image', content: 'http://www.laconfreriedumog.fr/logo.png' },
          { name: 'twitter:image:alt', content: 'Logo' }
      ],
      link: [
          { rel: 'icon', type: 'image/x-icon', href: '/logo.ico' }
      ]
    },

    mounted () {

    }
  }
</script>
