<template>
  <div class="loading" id="loading" v-if="loading">
      <div class="loading__content">
          <div class="loading__content--info">
              <img src="~/assets/loader.gif"/>
              Veuillez patienter Kuppo...
          </div>
      </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    loading: false
  }),
  methods: {
    start () {
      this.loading = true
    },
    finish () {
      this.loading = false
    }
  }
}
</script>
