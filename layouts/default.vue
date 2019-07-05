<template>
  <nuxt/>
</template>

<style lang="scss">
  @import '../assets/all';
</style>

<script>
  export default {
    data() {
      return {
        structuredData: {
          "@context": "http://schema.org",
          "@type": "VideoGame",
          // More structured data...
        },
      }
    },

    head () {
      return {
        script: [
          { type: 'application/ld+json', src: JSON.stringify(this.structuredData) }
        ]
      }
    }
  }
</script>

