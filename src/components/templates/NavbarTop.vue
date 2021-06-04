<template>
  <div>
    <b-navbar toggleable="lg" type="dark" class="custom-bg">
      <div class="container">
        <router-link :to="{ name: 'flight-page'}" custom v-slot="{ href }">
          <b-navbar-brand :href="href">AIU</b-navbar-brand>
        </router-link>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <router-link :to="{ name: 'flight-page'}" custom v-slot="{ href }">
              <b-nav-item :href="href">Flights</b-nav-item>
            </router-link>
          </b-navbar-nav>
          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
            <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: 'NavbarTop'
}
</script>

<style scoped>
.custom-bg {
  background: #0f5286;
}
</style>
