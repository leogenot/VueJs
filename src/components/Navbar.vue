<template>
<div class="nav-bar">
<b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#">Home</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item href="#">Favourites</b-nav-item>
      </b-navbar-nav>

      <!-- Right aligned nav items  -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-form>

        <b-nav-item-dropdown text="Sort by" right>
          <b-dropdown-item href="#">A to Z</b-dropdown-item>
          <b-dropdown-item href="#">Z to A</b-dropdown-item>
          <b-dropdown-item href="#">From most alcohol to least</b-dropdown-item>
          <b-dropdown-item href="#">From least alcohol to most</b-dropdown-item>
        </b-nav-item-dropdown>

      </b-navbar-nav>
    </b-collapse>
  </b-navbar> 

  </div>

</template>

<script>
export default {
  name: "nav-bar"
  
};
</script>

<style>

</style>
