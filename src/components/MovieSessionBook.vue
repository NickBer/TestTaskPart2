<template>
  <div>
    <button
      type="button"
      class="btn btn-primary"
      data-toggle="modal"
      data-target="#bookModal"
    >Book ticket</button>
    <div class="modal fade" tabindex="-1" id="bookModal" role="dialog">
      <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Book ticket ({{session.ticketPrice}} UAH)</h5>
            <button
              @click="reservationCode = ''"
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div v-if="!reservationCode" class="container-fluid">
              <div class="m-auto" style="width:450px;">
                <div
                  v-for="(row,ind) in session.places"
                  :key="ind"
                  class="btn-group"
                  role="group"
                  aria-label="row"
                >
                  <button
                    v-for="place in row"
                    :key="place._id"
                    @click="selectPlace(session._id,ind,place.position,place._id)"
                    type="button"
                    :class="{'btn  m-1 btn-secondary btn-light':true,
                    'btn-dark':place.isFree||selectedPlaceId===place._id||bookedPlaces.includes(place._id)
                    }"
                    :disabled="place.isFree||bookedPlaces.includes(place._id)"
                    data-toggle="tooltip"
                    data-placement="top"
                    :title="`row: ${ind+1} \nplace: ${place.position+1}`"
                  >{{place.position+1}}</button>
                </div>
              </div>
            </div>
            <p v-if="reservationCode">Your reservation code:{{reservationCode}}</p>
          </div>
          <div class="modal-footer">
            <button
              @click="bookTicket()"
              v-if="!reservationCode"
              type="button"
              class="btn btn-primary"
            >Buy</button>
            <button
              @click="reservationCode = ''"
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >Close</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ky from "ky";
export default {
  props: {
    session: Object
  },
  data() {
    return {
      selectedPlace: {},
      bookedPlaces: [],
      selectedPlaceId: "",
      reservationCode: ""
    };
  },
  methods: {
    selectPlace(movieShow_id, row_id, place_position, placeId) {
      this.selectedPlace = {
        movieShow_id,
        row_id,
        place_position,
        isFree: true
      };
      this.selectedPlaceId = placeId;
    },
    bookTicket() {
      (async () => {
        this.reservationCode = await ky
          .post("https://cinema-api-test.herokuapp.com/bookingPlace", {
            json: this.selectedPlace
          })
          .text();
        this.bookedPlaces.push(this.selectedPlaceId);
      })();
    }
  }
};
</script>