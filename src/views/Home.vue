<template>
  <div class="home">
    <!-- WRAPPER -->
    <div class="wrapper">

      <!-- HOME -->
      <section class="module module-header bg-dark bg-dark-50" data-background="/assets/images/post-1.jpg">

        <div class="container">

          <!-- MODULE TITLE -->
          <div class="row">
            

            <div class="col-sm-6 col-sm-offset-3">

              <h1 class="module-title font-alt align-center">Directory</h1>

              <div class="module-subtitle font-inc align-center">
                Black owned restaurants in the bay area. Search, filter by location, and filter by cuisine type.
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

      <!-- BLOG GRID -->
      <section class="p-t-40 p-b-0">

        <div class="container">
          
          <div class="form-group">
            <div class="row">
              <div class="col-xs-12">
                <input type="text" class="form-control" placeholder="search" v-model="searchFilter">
              </div>
            </div>
            <br>
            
            <div class="row multi-columns-row">
              
              <div class="col-xs-12 col-sm-5">
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
              <div class="col-xs-12 col-sm-5">
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
              <div class="col-xs-12 col-sm-2 text-center">
                <button v-on:click="resetFilters()"class="btn btn-d btn-round">Reset</button>
              </div>
            </div>      
          </div>
          
          
          <div class="p-t-40 row multi-columns-row post-columns">

          <!-- POST ITEM -->
          <div v-for="restaurant in filterBy(filterBy(filterBy(restaurants, searchFilter), cuisineType), locationType)" class="col-xs-6 col-md-3 col-lg-3">
            <div class="post">
              <div class="post-header">
                <h2 class="post-title font-alt"><a target="_blank" :href="restaurant.website">{{ restaurant.name }}</a></h2>
                <div class="post-meta font-inc">
                  <a v-on:click="locationType = restaurant.location">{{ restaurant.location }}</a> | {{ restaurant.cuisine }}
                </div>
              </div>
              <div class="post-entry">
                <p>Offering: {{ restaurant.service }}</p>
                <p v-if="restaurant.phone">Phone: <a :href="`tel:${restaurant.phone}`">{{ restaurant.phone }}</a></p>
                <p v-else>No phone provided</p>
                <p v-if="restaurant.instagram">Insta: {{ restaurant.instagram }}</p>
              </div>
              <div v-if="restaurant.website" class="post-more font-inc">
                <a :href="restaurant.website" target="_blank" class="more-link">Website</a>
              </div>
              <div v-else class="post-more font-inc"><a :href="`http://www.google.com/search?q=${restaurant.name}`" target="_blank">Google Search</a></div>
            </div>
          </div>
          <!-- /POST ITEM -->

          </div>

        </div>

      </section>
      <!-- /BLOG GRID -->

    </div>
    <!-- /WRAPPER -->
  
  </div>
</template>

<style>
.bg-dark a#plug:hover,
.bg-dark a#plug:focus {
  color: darkviolet;
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
  created: function() {
    axios
      .get("https://black-owned-restaurants-api.herokuapp.com/api/restaurants")
      .then(response => {
        console.log("restaurants index", response);
        this.restaurants = response.data;
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