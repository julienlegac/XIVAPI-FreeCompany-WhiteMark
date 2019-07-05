<template>
  <div id="app" class="row">
    <div class="character-info">
      <div class="character-info__container">
        <Member/>
      </div>
    </div>
  </div>
</template>

<script>
  import Member from '~/components/Member.vue'

  export default {
    name: 'app',
    layout: 'default',
    components: {
      Member,
    },

    mounted () {
      this.$nextTick(() => {
        this.$nuxt.$loading.start()

        setTimeout(() => this.$nuxt.$loading.finish(), 500)
      })
    }
  }
</script>
