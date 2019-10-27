<template>
     <div class="d-flex flex-column">

        <div class="text-center mt-5">
            <v-btn @click="this.handleLoad" outlined text>Load Hotels</v-btn>
        </div>

        <v-container v-if="showData">
        <v-flex  
            v-for="(hotel,idHotel) in hotels"
            :key="idHotel"
            
        >
<!-- ==================== HOTEL CONTAINER ==================== -->
            <v-card
                class="mx-auto mb-2"
                max-width="98%"
               
                outlined
            >
                <div class="d-flex flex-row">
                    <div :aspect-ratio="16/9">
                        <v-img
                            width="200"
                            :src="hotel.images[0]"
                        ></v-img>
                    </div>

                    <div>
                        <v-list-item three-line>
                            <v-list-item-content>
                                <v-layout row justify-space-between>
                                    <div class="title pl-3">{{ hotel.name }}</div>
                                    <div class="text-center mr-5"> 
                                            <v-rating v-model="rating" size="16" :dense="dense" background-color="black" color="black"
                                            :show-rating="true" @current-rating="showCurrentRating"
                                            :readonly="readonly"
                                            >
                                               {{ hotel.stars }}                                             
                                            </v-rating>
                                    </div>
                                </v-layout> 
                                <v-list-item-title class="subtitle-2 mb-1">{{ hotel.city }} - {{ hotel.country }}</v-list-item-title>
                            
                                <v-list-item-subtitle>{{ hotel.description }}</v-list-item-subtitle>
                            </v-list-item-content>
                        </v-list-item>

                        <div class="d-flex flex-row justify-space-around align-center">
                            <v-card-actions>
                                <v-btn
                                   @click="this.handleReview(hotels.id)">{{ button.text }}
                                </v-btn>
                            </v-card-actions>
                        
                            <h4 class="align-center ">{{ hotel.price }} €</h4>
                            <!-- <p class="align-center ">{{ hotel.date_start }} - {{ hotel.date_end }}</p> -->
                        </div>
                    </div>
   
                </div>

<!-- ==================== REVIEW CONTAINER ==================== -->
                 <v-card>
                    <v-flex
                        v-for="review in reviews"
                        :key="review.hotel_id"
                    >

                        <v-card
                            v-if="show"
                            class="mx-auto"
                            max-width="800"
                            outlined
                        >

                        <v-expand-transition>
                            <v-show="show">

                            <div class="d-flex flex-row">
                                    <div>
                                        <p>{{ review.positive }}</p>
                                    </div>

                                    <div>
                                        <h5>{{ review.name }}</h5>
                                        <h6>{{ review.comment }}</h6>
                                    </div>
                            </div>
                        </v-expand-transition>

                        </v-card>
                    </v-flex>
                </v-card>
            </v-card>
        </v-flex>
        </v-container>
          <!-- <v-spacer></v-spacer> -->
           

<!-- ===================++= ERROR CONTAINER =========++=========== -->
        <v-container v-if="errorMsg">
                <h5 class="errorfield">An error occured</h5>
        </v-container>

        <!-- button to top -->
        <back-to-top bottom="5px" right="100px" visibleoffset="120px">
            <button type="button" class="btn btn-info btn-to-top">
                <i class="fas fa-angle-double-up"></i>
            </button>
        </back-to-top>
    </div>
</template>


<script>
import axios from 'axios';
import BackToTop from "vue-backtotop";

export default {
    data() {
        return {
            hotels: [],
            reviews: [],
            errors: [],
            show: false,
            id: "",
            hotel_id: "",
            button: { text: "Show Reviews" },
            dense: true,
            rating: 0,
            readonly: true,
            showData: false,
            errorMsg: false
        }
    },
    components: { BackToTop },

    // props: ["id: hotel_id"],

    methods: {
        async handleLoad() {
            try {
            const response = await axios.get('http://fake-hotel-api.herokuapp.com/api/hotels')
            this.hotels = response.data;
            this.showData = true
            this.errorMsg = false
            console.log(this.hotels)
            return this.hotels;
            } catch (error) {
                this.showData = false
                this.errorMsg = true
            }
        },

// TODO: how to load ratings into v-rating ???
        showCurrentRating() {
            this.hotel.stars = this.stars
            this.currentRating = this.hotel.stars
        },

// TODO: how to load review component and send ID ???
        handleReview(id) {  
            this.show = !this.show;
            this.button.text = !this.show ? "Show Reviews" : "Hide Reviews";
            
            this.hotels_id = id;
            console.log(this.hotel_id)
            console.log(event.target.id)

            axios.get('http://fake-hotel-api.herokuapp.com/api/reviews?hotel_id=${this.hotel_id}')
                .then(response => {
                    this.reviews = response.data
                    console.log(this.reviews)
                })
                .catch( error => { console.log(error) } );
        }
    }
}



</script>

<style>
.errorfield {
    width: 95%;
    margin: auto;
    color: #5d5757;
    background-color: #c7c7c7;
    padding: 5px;
    border: 1px solid darkgrey;
}

.btn-to-top {
  width: 40px;
  height: 40px;
  border: 1px solid lightgrey;
  border-radius: 50%;
  font-size: 22px;
  line-height: 22px;
}
</style>