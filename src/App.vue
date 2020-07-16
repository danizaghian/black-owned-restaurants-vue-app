<template>
  <div id="app">
    <!-- NAVIGATION -->
    <nav id="totop" class="navbar navbar-custom navbar-transparent navbar-fixed-top" role="navigation">

      <div class="container">
    
        <div class="navbar-header">
          <!-- <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#custom-collapse">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button> -->
          <router-link class="navbar-brand" to="/">BABOR</router-link>
        </div>
    
        <!-- <div class="collapse navbar-collapse" id="custom-collapse">
    
          <ul class="nav navbar-nav navbar-right">
    
            <li><router-link to="/">Directory</router-link></li>
    
          </ul>
        </div> -->
    
      </div>

    </nav>
    <!-- /NAVIGATION -->

    <!-- WRAPPER -->
	  <div class="wrapper">
      <router-view/>

      <!-- FOOTER -->
      <footer class="footer">
      
        <div class="container">
      
          <div class="row">
      
            <div class="font-inc align-center">
              <a id="plug" href="https://www.linkedin.com/in/danizaghian/">BY Dani Zaghian</a>
            </div>
      
          </div>
      
        </div>
      
      </footer>
      <!-- /FOOTER -->

    </div>
	  <!-- /WRAPPER -->
    
  </div>
</template>


