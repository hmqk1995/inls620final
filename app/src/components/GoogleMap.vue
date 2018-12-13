<template>
  <div class="map">
      <div id="map"></div>
  </div>
</template>

<script>
import mapLoader from 'google-maps'
import * as d3sparql from 'd3-sparql'
export default {
  name: 'Map',
  data() {
    return {
      lat: 35.9120729,
      lng: -79.0512295,
      zoom: 16,
      map: null,
    }
  },
  mounted() {
    let query = `
      PREFIX : <https://kunq.me/incidents/>

      SELECT ?name ?lat ?long
      WHERE {
        ?subject :offenseName ?name ;
                 :lat ?lat ;
                 :long ?long .
      }
    `

    new Promise((resolve, reject) => {
      d3sparql.sparql(
        'http://localhost:3030/incidents/query', 
        query, 
        (error, data) => {
          if (error) throw error
          console.log(data)
          resolve(data)
        })

    }).then(data => {
        // The location of Chapel Hill
        mapLoader.KEY = 'AIzaSyAzVp_JinWqYIKMBLv6FBm9HjXB--gX9Mw'
        mapLoader.load(google => {
          this.map = new google.maps.Map(
            this.$el.querySelector('#map'), {
              zoom: this.zoom, 
              center: {
                lat: this.lat,
                lng: this.lng,
              }
            })
          data.forEach(element => {
            let marker = new google.maps.Marker({
              position: {
                lat: parseFloat(element.lat), 
                lng: parseFloat(element.long)
              },
              map: this.map,
              clickable: true,
            })
            let infowindow = new google.maps.InfoWindow({
              content: element.name,
            });
            marker.addListener('click', function() {
              infowindow.open(this.map, marker);
            });
          });
        })
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#map {
  height: calc(100vh - 100px);
}
</style>
