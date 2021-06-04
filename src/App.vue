<template>
  <div>
    <navbar-top />
    <router-view />
  </div>
</template>

<script>
import VueRouter from 'vue-router'
import NavbarTop from './components/templates/NavbarTop'
import FlightPage from './components/pages/public/FlightPage'
import Page404 from './components/pages/common/Page404'
import TicketPage from './components/pages/public/TicketPage'

const router = new VueRouter({
  routes: [
    // Flight
    { path: '/', name: 'flight-page', component: FlightPage },

    // Ticket
    { path: '/flight/:flightId/ticket', name: 'ticket-page', component: TicketPage },

    // Page 404
    { path: '/404', name: 'notfound', component: Page404 },
    { path: '*', redirect: { name: 'notfound' } }
  ]
})

export default {
  name: 'App',
  router,
  components: {
    NavbarTop
  }
}
</script>

<style lang="scss">
body {
  background: #ECECECFF;
}
.text-black-75 {
  color: #555;
}
</style>
