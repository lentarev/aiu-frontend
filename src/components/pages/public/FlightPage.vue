<template>
  <div>
    <!-- Spinner -->
    <div class="m-2">
      <div class="preloader text-center" slot="placeholder" v-if="!isLoadedContent">
        <b-spinner class="m-4" label="Loading..." style="color: #cccccc"></b-spinner>
      </div>
    </div><!-- Spinner -->
      <div v-if="isLoadedContent">
          <div class="container mt-2">
            <!-- Loop. List of flights-->
            <div v-for="item in content" :key="item.id">
              <div class="row pb-1 row-hover" v-on:click="gotoTicketPage(item.id)"> <!-- main row -->
                <div class="col-md bg-white rounded p-1 pl-2">
                  <h5 class="p-2" variant="light">{{ item.airline }}</h5>
                  <div class="container">
                    <div class="row">
                      <!-- departure -->
                      <div class="col-md bg-white rounded p-1 pl-2">
                        <div class="p-2 m-1 text-black-75">
                          <div>
                            <b-badge class="p-2 m-1" variant="light"> {{ getDateFormat(item.departureTime.date)  }} </b-badge>
                            <b-badge class="p-2 m-1" variant="light">{{ getTimeFormat(item.departureTime.date)  }} </b-badge>
                            <p class="ml-1">{{ item.departureCity }}</p>
                          </div>
                        </div>
                      </div> <!-- // departure -->
                      <!-- arrival -->
                      <div class="col-md bg-white rounded p-1 pl-2">
                        <div class="p-2 m-1 text-black-75">
                          <div>
                            <b-badge class="p-2 m-1" variant="success"> {{ getDateFormat(item.arrivalTime.date)  }} </b-badge>
                            <b-badge class="p-2 m-1" variant="success">{{ getTimeFormat(item.arrivalTime.date)  }} </b-badge>
                            <p class="ml-1">{{ item.arrivalCity }}</p>
                          </div>
                        </div>
                      </div> <!-- // arrival -->
                      <!-- price -->
                      <div class="col-md bg-white rounded p-1 pl-2">
                        <div class="p-2 m-1 text-black-75">
                          <h3>
                            <b-badge class="p-2 m-1" variant="primary">{{ item.ticketPrice }} руб.</b-badge>
                          </h3>
                        </div>
                      </div> <!-- // price -->
                    </div>
                  </div>
                </div>
              </div> <!-- // main row -->
            </div>
          </div>
      </div>
  </div>
</template>

<script>
import Axios from 'axios'

export default {
  name: 'FlightPage',
  metaInfo: {
    title: 'AIU',
    titleTemplate: '%s | Welcome',
    htmlAttrs: {
      lang: 'en',
      amp: true
    }
  },
  data () {
    return {
      content: null,
      isLoadedContent: null
    }
  },
  mounted () {
    this.getFlights()
  },
  methods: {
    /**
     * getFlights
     * @returns {Promise<void>}
     */
    async getFlights () {
      const REST_API_URL = process.env.VUE_APP_REST_API_HOST + '/api/v1/flights'

      await Axios.get(REST_API_URL).then((response) => {
        this.content = response.data.json
        this.isLoadedContent = true
      }).catch(error => {
        // ERROR
        if (error.response) {
          // клиент получил ответ об ошибке (5xx, 4xx)
          console.log(error.response)
        } else if (error.request) {
          // клиент никогда не получал ответ
          console.log(error.request)
        }
      })
    },

    /**
     *  @param flightId
     */
    gotoTicketPage (flightId) {
      this.$router.push({ name: 'ticket-page', params: { flightId: flightId } })
    },

    /**
     * @param date
     * @returns {string}
     */
    getDateFormat (date) {
      const fullDate = date.split(' ')
      const dateArray = fullDate[0].split('-')
      const year = dateArray[0]
      const month = dateArray[1]
      const day = dateArray[2]
      return year + ' / ' + month + ' / ' + day
    },
    /**
     * @param date
     * @returns {string}
     */
    getTimeFormat (date) {
      const fullDate = date.split(' ')
      const timeArray = fullDate[1].split(':')
      const hour = timeArray[0]
      const minute = timeArray[1]
      return hour + ':' + minute
    }
  }
}
</script>

<style scoped>
.row-hover :hover {
  cursor: pointer;
}
</style>
