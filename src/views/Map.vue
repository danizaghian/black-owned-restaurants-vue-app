<template>
  <div class="map">
    <!-- WRAPPER -->
    <div class="wrapper">

      <!-- HOME -->
      <section class="module module-header bg-dark bg-dark-50" data-background="/assets/images/section-7.jpg">

        <div class="container">

          <!-- MODULE TITLE -->
          <div class="row">
            

            <div class="col-sm-6 col-sm-offset-3">

              <h1 class="module-title font-alt align-center">Map</h1>

              <div class="module-subtitle font-inc align-center">
                Bay area black owned restaurants in a filterable, searchable map view.
              </div>

            </div>

          </div>
          <!-- /MODULE TITLE -->

        </div>

      </section >
      <!-- /HOME -->


      <!-- BLOG GRID -->
      <section class="p-t-40 p-b-40">

        <div class="container">
       
          <div class="row">
            <div class="col-xs-12">
              <input type="text" class="form-control" placeholder="search" v-model="searchFilter">
            </div>
          </div>
          <br>
          
          <div class="row multi-columns-row">
            
            <div class="col-xs-12 col-sm-6">
              <select class="form-control" v-model="cuisineType">
                <option disabled value="">Select cuisine</option>
                <option>Caribbean</option>
                <option>African</option>
                <option>Southern</option>
                <option>American</option>
                <option>Wine</option>
                <option>Coffee</option>
                <option>Barbecue</option>
                <option>Cajun</option>
                <option>Dessert</option>
                <option>Ethiopian</option>
                <option>Vegan</option>
              </select>
            </div>
            <div class="col-xs-12 col-sm-6">
              <select class="form-control" v-model="locationType">
                <option disabled value="">Select location</option>
                <option>Alameda</option>
                <option>Albany</option>
                <option>Antioch</option>
                <option>Berkeley</option>
                <option>Concord</option>
                <option>Daly City</option>
                <option>Emeryville</option>
                <option>Fairfield</option>
                <option>Fremont</option>
                <option>Hayward</option>
                <option>Healdsburg</option>
                <option>Oakland</option>
                <option>Palo Alto</option>
                <option>Richmond</option>
                <option>San Francisco</option>
              </select>
            </div>
            
          </div> 
          <br>

          <div class="row">
            <div class="col-xs-6">
              <button v-on:click="filterMarkers()"class="btn btn-d btn-round">Filter</button>
            </div> 
            <div class="col-xs-6 text-right">
              <button v-on:click="resetFilters()"class="btn btn-d btn-round">Reset</button>
            </div> 
          </div>     
     

        </div>

      </section>
      <!-- /BLOG GRID -->

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
      locationType: "",
      map: {}
    };
  },
  created: function() {},
  mounted: function() {
    this.initializeMap();
  },
  methods: {
    resetFilters: function() {
      this.initializeMap();
      this.searchFilter = "";
      this.cuisineType = "";
      this.locationType = "";
    },
    initializeMap: function() {
      axios
        .get(
          "https://black-owned-restaurants-api.herokuapp.com/api/restaurants"
        )
        .then(response => {
          console.log("restaurants index", response);
          this.restaurants = response.data;
          mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_API_KEY;
          this.map = new mapboxgl.Map({
            container: "map", // container id
            style: "mapbox://styles/dzaghian/ck1uzqv8o7muc1do8008p7mst", // stylesheet location
            center: [-122.4194, 37.7749], // starting position [lng, lat]
            zoom: 9 // starting zoom
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
                .addTo(this.map);
            }
          });
        });
    },
    filterMarkers: function() {
      this.map = new mapboxgl.Map({
        container: "map", // container id
        style: "mapbox://styles/dzaghian/ck1uzqv8o7muc1do8008p7mst", // stylesheet location
        center: [-122.4194, 37.7749], // starting position [lng, lat]
        zoom: 9 // starting zoom
      });

      this.filterBy(
        this.filterBy(
          this.filterBy(this.restaurants, this.cuisineType),
          this.locationType
        ),
        this.searchFilter
      ).forEach(restaurant => {
        if (restaurant.address) {
          // create the popup
          var popup = new mapboxgl.Popup({ offset: 25 }).setText(
            restaurant.name
          );
          // create the marker
          var marker = new mapboxgl.Marker()
            .setLngLat([restaurant.long, restaurant.lat])
            .setPopup(popup) // sets a popup on this marker
            .addTo(this.map);
        }
      });
    }
  }
};
</script>