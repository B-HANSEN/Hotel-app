<template>
     <div class="d-flex flex-column">

        <div class="text-center mt-5">
            <v-btn @click="this.handleLoad" outlined text>Load Hotels</v-btn>
        </div>

        <v-container v-if="!noData">
        <v-flex  
            v-for="(hotel,idHotel) in hotels"
            :key="idHotel"
        >
<!-- ==================== HOTEL CONTAINER ==================== -->
            <v-card
                class="mx-auto"
                max-width="800"
                outlined
            >
                <div class="d-flex flex-row">
                    <div>
                        <v-img
                            height="200"
                            width="200"
                            :src="hotel.images[0]"
                        ></v-img>
                    </div>

                    <div>
                        <v-list-item three-line>
                            <v-list-item-content>
                                <v-layout row justify-space-between>
                                    <div class="overline">{{ hotel.name }}</div>
                                    <div class="text-center mr-5"> 
                                            <v-rating v-model="rating" size="16" dense="true" background-color="black" color="grey">{{ hotel.stars }}                                                
                                            </v-rating>
                                    </div>
                                </v-layout> 
                                <v-list-item-title class="headline mb-1">{{ hotel.city }} - {{ hotel.country }}</v-list-item-title>
                            
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
           

        <v-container v-else>
            <div class="text-xs-center">
            <h2>An error occured</h2>
            </div>
        </v-container>

    </div>
</template>


<script>
import axios from 'axios';

export default {
    data() {
        return {
            hotels: [],
            reviews: [],
            errors: [],
            show: false,
            id: "",
            hotel_id: "",
            noData: false,
            button: {
                text: "Show Reviews"
            }
        }
    },

    // props: ["id: hotel_id"],

    methods: {
        handleLoad() {
            axios.get('http://fake-hotel-api.herokuapp.com/api/hotels')
                        .then(response => {
                            this.hotels = response.data
                            console.log(this.hotels)
                            console.log(this.hotels[0].id)
                            if (response.data.Response === 'True') {
                                this.movieResponse = response.data.Search
                                this.noData = true
                            } else {
                                this.noData = false
                            }
                        })
                        .catch( error => { console.log(error) } );
        },
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
</style>