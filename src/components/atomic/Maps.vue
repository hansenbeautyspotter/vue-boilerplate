<template lang="html">
  <gmap-map
    :center="{lat: 52.520645, lng: 13.409779}"
    :zoom="12"
    style="width: 100%; height: 100%; position: absolute; left:0; top:0;">
    <gmap-marker v-for="marker in data.markers" :key="marker.id" :position="marker.position" @click="toggleInfoWindow(marker, marker.id)" :clickable="true" :draggable="false"></gmap-marker>
    <gmap-info-window :options="util.infoOptions" :position="util.infoWindowPos" :opened="util.infoWinOpen" :content="util.infoContent" @closeclick="util.infoWinOpen=false"></gmap-info-window>
  </gmap-map>
</template>

<script>
  import Vue from 'vue'
  import * as VueGoogleMaps from 'vue2-google-maps'
  import eventHub from '@/util/EventHub'
  import _ from 'lodash'

  Vue.use(VueGoogleMaps, {
    load: {
      key: "AIzaSyDRfbsBRInMl825wIo0AxF-GGGgh1tThCE"
    }
  });


  export default {
    name: 'maps',
    data () {
      return {
        data: {
          markers: []
        },
        util: {
          infoContent: '',
          infoWindowPos: {
            lat: 0,
            lng: 0
          },
          infoWinOpen: false,
          currentMidx: null,
          infoOptions: {
            pixelOffset: {
              width: 0,
              height: -35
            }
          }
        }
      }
    },
    methods: {
      createMarker: function(markerId, geoData) {
        let tempObj = {
          id: markerId,
          position: geoData
        }

        if(!_.find(this.data.markers, tempObj)) {
          this.data.markers.push(tempObj);
        }
      },
      toggleInfoWindow: function(marker, idx) {
          this.util.infoWindowPos = marker.position;
          this.util.infoContent = "This is tooltip for " + marker.id;

          if (this.util.currentMidx == idx) {
            this.util.infoWinOpen = !this.util.infoWinOpen;
          } else {
            this.util.infoWinOpen = true;
            this.util.currentMidx = idx;
          }
      }
    },
    mounted() {
      eventHub.$on('maps-create-marker', (data) => {
        this.createMarker(data.markerId, data.geoData);
      });
    }
  }
</script>

<style scoped lang="scss">
</style>
