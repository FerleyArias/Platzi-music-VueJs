<template lang="pug">
  main
    transition(name="move")
      pm-notification(v-show="showNotification" :error="error")
        p(slot="body" v-if="error") No se encontraron resultados
        p(slot="body" v-else) se han encontrado {{tracks.length}} canciones

    transition(name="move")
      pm-loader(v-show="isLoading")
    section.section(v-show="!isLoading")
      nav.nav
        .container
          input.input.is-large(
            type="text"
            placeholder="Buscar canciones"
            v-model="searchQuery"
            @keyup.enter="search"
          )
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is-danger.is-large &times
      .container
        p
          small {{ searchMessage }}

      .container.results
        .columns.is-multiline
          .column.is-one-quarter(v-for="t in tracks")
            pm-track(
              v-blur="t.preview_url"
              :class="{ 'is-active': t.id === selectedTrack }"
              :track="t"
              @select="setSelectedTrack")
</template>

<script>
import trackService from '@/services/track'

import PmTrack from '@/components/Track'

import PmLoader from '@/components/shared/Loader'
import PmNotification from '@/components/shared/Notification'

export default {
  name: 'app',

  components: { PmLoader, PmTrack, PmNotification },

  data () {
    return {
      searchQuery: '',
      tracks: [],

      isLoading: false,
      showNotification: false,
      error: false,

      selectedTrack: ''
    }
  },

  methods: {
    search () {
      if (!this.searchQuery) { return }

      this.isLoading = true

      trackService.search(this.searchQuery)
        .then(res => {
          this.error = res.tracks.total === 0
          this.showNotification = true
          this.tracks = res.tracks.items
          this.isLoading = false
        })
    },

    setSelectedTrack (id) {
      this.selectedTrack = id
    }
  },

  watch: {
    showNotification () {
      if (this.showNotification) {
        setTimeout(() => {
          this.showNotification = false
        }, 3000)
      }
    }
  },

  computed: {
    searchMessage () {
      return `Encontrados: ${this.tracks.length}`
    }
  }
}
</script>

<style lang="scss">
.results {
  margin-top: 50px;
}

.is-active {
  border: 3px solid #23d160
}
</style>
