<template>
  <div id="app" class="row">
    <ListMembers/>
  </div>
</template>

<script>
  import ListMembers from '~/components/ListMembers.vue'

  export default {
    name: 'app',
    layout: 'default',
    components: {
      ListMembers,
    },

    head: {
      title: 'La confrérie du mog - Final Fantasy XIV, Cerberus',
      meta: [
        { charset: 'utf-8' },
        { name: 'viewport', content: 'width=device-width, initial-scale=1' },
        { hid: 'description', name: 'description', content: '"La Confrérie du Mog" est une compagnie libre du jeu FINAL FANTASY XIV. Rejoignez-nous sur Cerberus !' },

        // Open Graph
        { name: 'og:title', content: 'La confrérie du mog - Final Fantasy XIV, Cerberus'},
        { name: 'og:description', content: '"La Confrérie du Mog" est une compagnie libre du jeu FINAL FANTASY XIV. Rejoignez-nous sur Cerberus !' },
        { name: 'og:type', content: 'website' },
        { name: 'og:url', content: 'http://www.laconfreriedumog.fr' },
        { name: 'og:image', content: 'https://wwww.laconfreriedumog.fr/logo.png' },
        // Twitter Card
        { name: 'twitter:card', content: 'summary' },
        { name: 'twitter:site', content: '@Ozzlo_' },
        { name: 'twitter:title', content: 'La confrérie du mog - Final Fantasy XIV, Cerberus' },
        { name: 'twitter:description', content: '"La Confrérie du Mog" est une compagnie libre du jeu FINAL FANTASY XIV. Rejoignez-nous sur Cerberus !' },
        { name: 'twitter:image', content: 'https://wwww.laconfreriedumog.fr/logo.png' },
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
