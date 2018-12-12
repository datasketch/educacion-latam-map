<template>
  <div id="app" :class="{ column: loading }">
    <Loader
      v-if="loading"
      color="#004e70"
      :size="40" />
    <Map
      v-else
      :center="[4.6097100, -74.0817500]"
      :points="points"
      :values="values"
      tile="https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png" />
  </div>
</template>

<script>
import Map from '@/components/Map'
import Loader from '@/components/Loader'
import { getSpreadsheetData } from '@/services/'

export default {
  components: { Map, Loader },
  name: 'App',
  data () {
    return {
      points: [],
      values: [],
      loading: true
    }
  },
  created () {
    getSpreadsheetData().then(({ points, values }) => {
      this.points = points
      this.values = values
      this.loading = false
    })
  }
}
</script>

<style>
*,
*:before,
*:after {
  box-sizing: inherit;
  margin: 0;
  outline: none;
}

html,
body {
  box-sizing: border-box;
  height: 100%;
  font-family: 'Lato', sans-serif;
  font-size: 10px;
  width: 100%;
}

#app {
  height: 100%;
}
</style>
