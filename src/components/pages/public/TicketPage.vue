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
          <div class="row pb-1"> <!-- main row -->
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

          <!-- Loop. List of seats-->
          <div v-for="ticket in item.tickets" :key="ticket.id">
            <div class="row pb-1 row-hover"> <!-- sub row -->
              <div class="col-md bg-white rounded p-3 pl-2">
                <div class="container">
                  <div class="row">
                    <div class="col-md m-1">
                      <b-badge variant="light">Место № {{ ticket.placeNumber }}</b-badge>
                    </div>
                    <div class="col-md m-1">
                      <b-button v-if="!ticket.buy" variant="outline-primary" size="sm" v-on:click="setBuy(ticket.id)">Купить</b-button>
                      <b-button v-if="ticket.buy" variant="outline-danger" size="sm" v-on:click="setBuy(ticket.id)">Возврат</b-button>
                    </div>
                    <div class="col-md m-1">
                      <b-button v-if="!ticket.buy" size="sm" variant="light" v-on:click="setBookYourPlace(ticket.id)">
                        <span v-if="!ticket.bookYourPlace">Забронировать</span>
                        <div v-if="ticket.bookYourPlace" class="bg-danger pl-1 pr-1" style="color: white;">Забронирован</div>
                      </b-button>
                      <b-button v-if="ticket.buy" variant="outline-secondary" disabled size="sm" v-on:click="setBookYourPlace(ticket.id)">Недоступно</b-button>
                    </div>
                  </div>
                </div>
              </div>
            </div> <!-- // sub row -->
          </div> <!-- // Loop. List of seats-->
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import Axios from 'axios'

export default {
  name: 'TicketPage',
  metaInfo: {
    title: 'AIU',
    titleTemplate: '%s | Ticket',
    htmlAttrs: {
      lang: 'en',
      amp: true
    }
  },
  data () {
    return {
      content: null,
      isLoadedContent: null,
      flightId: null,
      buy: null,
      buyError: false,
      bookYourPlaceError: false
    }
  },
  mounted () {
    this.flightId = Number(this.$route.params.flightId)
    this.getTicket()
  },
  methods: {
    /**
     * getTicket
     * @returns {Promise<void>}
     */
    async getTicket () {
      const REST_API_URL = process.env.VUE_APP_REST_API_HOST + '/api/v1/ticket/' + this.flightId

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
     * setBuy
     * @returns {Promise<void>}
     */
    async setBuy (ticketId) {
      const REST_API_URL = process.env.VUE_APP_REST_API_HOST + '/api/v1/buy/ticket'

      await Axios.put(REST_API_URL, { ticketId: ticketId }).then((response) => {
        this.buyError = response.data.json.error
        this.getTicket()
        // this.isLoadedContent = true
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
     * setBookYourPlace
     * @returns {Promise<void>}
     */
    async setBookYourPlace (ticketId) {
      const REST_API_URL = process.env.VUE_APP_REST_API_HOST + '/api/v1/book/your/place'

      await Axios.put(REST_API_URL, { ticketId: ticketId }).then((response) => {
        this.bookYourPlaceError = response.data.json.error
        this.getTicket()
        // this.isLoadedContent = true
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

</style>
