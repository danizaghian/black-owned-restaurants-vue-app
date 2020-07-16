<template>
  <div class="map">
    <!-- WRAPPER -->
    <div class="wrapper">

      <!-- HOME -->
      <section class="module module-header bg-dark bg-dark-50" data-background="/assets/images/post-1.jpg">

        <div class="container">

          <!-- MODULE TITLE -->
          <div class="row">
            

            <div class="col-sm-6 col-sm-offset-3">

              <h1 class="module-title font-alt align-center">Bay Area Black Owned Restaurants</h1>

              <div class="module-subtitle font-inc align-center">
                Interact with the map, search, and filter.
              </div>

              <div class="font-inc align-center">
                <a id="plug" href="https://www.linkedin.com/in/danizaghian/">BY Dani Zaghian</a>
              </div>

            </div>

          </div>
          <!-- /MODULE TITLE -->

        </div>

      </section >
      <!-- /HOME -->

      <div id="map"></div>

    </div>
    <!-- /WRAPPER -->
  
  </div>
</template>

<style>
#map {
  height: 600px;
  width: 100%;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      restaurants: [],
      searchFilter: "",
      cuisineType: "",
      locationType: ""
    };
  },
  created: function() {},
  mounted: function() {
    axios
      .get("https://black-owned-restaurants-api.herokuapp.com/api/restaurants")
      .then(response => {
        console.log("restaurants index", response);
        this.restaurants = response.data;
        mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_API_KEY;
        var map = new mapboxgl.Map({
          container: "map", // container id
          style: "mapbox://styles/dzaghian/ck1uzqv8o7muc1do8008p7mst", // stylesheet location
          center: [-122.4194, 37.7749], // starting position [lng, lat]
          zoom: 10 // starting zoom
        });
        this.restaurants.forEach(restaurant => {
          if (restaurant.address) {
            // create the popup
            var popup = new mapboxgl.Popup({ offset: 25 }).setText(
              restaurant.name
            );
            // create the marker
            var marker = new mapboxgl.Marker()
              .setLngLat([restaurant.long, restaurant.lat])
              .setPopup(popup) // sets a popup on this marker
              .addTo(map);
          }
        });
      });
  },
  methods: {
    resetFilters: function() {
      this.searchFilter = "";
      this.cuisineType = "";
      this.locationType = "";
    }
  }
};
</script>