<template>
  <div id="map" ref="container" :class="{ active: organizations.length }">
    <div :class="['aside', { active: organizations.length }]">
      <div class="aside__header line v-centered justify-between">
        <h3 class="aside__title" @click="goBack">
          {{ selected ? '&larr; ' + city : city }}
        </h3>
        <div class="close" @click="close" />
      </div>
      <div class="aside__content">
        <component
          :is="view"
          :organizations="organizations"
          :onclick="selectOrganization"
          :organization="selected">
        </component>
      </div>
    </div>
    <div ref="map"/>
  </div>
</template>

<script>
import 'leaflet/dist/leaflet.css'
import Organizations from '@/components/Organizations'
import Organization from '@/components/Organization'
import L from 'leaflet'

export default {
  name: 'Map',
  components: { Organizations, Organization },
  props: {
    tile: {
      type: String,
      required: true
    },
    center: {
      type: Array,
      required: true
    },
    minZoom: {
      type: Number,
      default: 3
    },
    maxZoom: {
      type: Number,
      default: 7
    },
    points: {
      required: true,
      type: Array
    },
    values: {
      required: true,
      type: Array
    }
  },
  data () {
    return {
      map: undefined,
      group: undefined,
      city: undefined,
      country: undefined,
      selected: undefined,
      organizations: [],
      styles: {
        global: {
          color: '#66bb6a',
          fillColor: '#66bb6a',
          fillOpacity: 0.5,
          opacity: 0.5,
          radius: 65000,
          weight: 1
        },
        active: {
          fillOpacity: 0.9,
          opacity: 0.9
        }
      }
    }
  },
  computed: {
    view () {
      return this.selected ? Organization : Organizations
    }
  },
  mounted () {
    this.initializeMap()
  },
  methods: {
    initializeMap () {
      this.map = L.map(this.$refs.map, {
        center: this.center,
        zoom: this.minZoom,
        minZoom: this.minZoom,
        maxZoom: this.maxZoom
      })

      L.tileLayer(this.tile).addTo(this.map)

      this.group = L.featureGroup()

      this.points.map(([city, country, lat, lng]) => {
        if (lat) {
          const point = L.circle([lat, lng], this.styles.global).bindTooltip(`${city}, ${country}`, { direction: 'top' })
          point.on('click', this.handleClickOnLayer)
          this.group.addLayer(point)
        }
      })

      this.group.addTo(this.map)
    },
    handleClickOnLayer ({ target }) {
      const { lat, lng } = target.getLatLng()
      this.group.setStyle(this.styles.global)
      target.setStyle(this.styles.active)
      this.organizations = this.values.filter(v => (+v.lat === lat) && (+v.lng === lng))
      this.city = this.organizations[0].city
      this.country = this.organizations[0].country
      this.map.fitBounds(target.getBounds())
    },
    selectOrganization (organization) {
      this.selected = organization
    },
    close () {
      this.organizations = []
      this.city = undefined
      this.selected = undefined
      this.group.setStyle(this.styles.global)
      this.map.setZoom(this.minZoom)
    },
    goBack () {
      if (!this.selected) {
        return
      }
      this.selected = undefined
    }
  }
}
</script>

<style>
#map,
.leaflet-container {
  height: 100%;
}

#map {
  transition: 0.5s ease 0.2s;
}

#map.active {
  transition-delay: 0s;
  padding-left: 350px;
}

.aside {
  background: #ffffff;
  height: 100%;
  left: 0;
  max-height: 100%;
  overflow-x: hidden;
  overflow-y: hidden;
  position: absolute;
  top: 0;
  transition: 0.5s ease 0.2s;
  width: 0px;
}

.aside.active {
  overflow-y: auto;
  width: 350px;
  transition-delay: 0s;
}

.aside::-webkit-scrollbar {
  width: 7px;
}

.aside::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.aside::-webkit-scrollbar-thumb {
  background: #888888;
}

.aside::-webkit-scrollbar-thumb:hover {
  background: #555555;
}

.aside__header {
  background: #ffffff;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
  font-size: 1.8rem;
  font-weight: 700;
  padding: 10px;
  position: sticky;
  top: 0;
}

.aside .aside__content,
.aside .aside__header {
  opacity: 0;
  transition: 0.25s ease 0s;
}

.aside.active .aside__content,
.aside.active .aside__header {
  opacity: 1;
  transition-delay: 0.5s;
}

.close {
  background-image: url(..//assets/images/close.png);
  background-position: center;
  background-size: cover;
  cursor: pointer;
  height: 15px;
  width: 15px;
}
</style>
