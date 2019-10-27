<template>
     <div class="d-flex flex-column">

        <v-btn text mb-5>Load Hotels</v-btn>

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
                                <div class="overline mb-4">{{ hotel.name }}</div>
                                <v-list-item-title class="headline mb-1">{{ hotel.city }} - {{ hotel.country}}</v-list-item-title>
                                <v-list-item-subtitle>{{ hotel.description }}</v-list-item-subtitle>
                            </v-list-item-content>
                        </v-list-item>

                        <div class="d-flex flex-row justify-space-around align-center">
                            <v-card-actions>
                                <v-btn
                                   @click="toggleButton">{{ button.text }}
                                </v-btn>
                            <!-- <v-spacer></v-spacer> -->
                            </v-card-actions>

                            <h4 class="align-center ">{{ hotel.price }} €</h4>
                            <!-- <p class="align-center ">{{ hotel.date_start }} - {{ hotel.date_end }}</p> -->
                        </div>
                    </div>
   
                </div>

                    <v-flex
                        v-for="(review,idHotel) in reviews"
                        :key="idHotel"
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
        </v-flex>
          <!-- <v-spacer></v-spacer> -->
           
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
            button: {
                text: "Show Reviews"
            }
        }
    },

    created() {
        axios.get('http://fake-hotel-api.herokuapp.com/api/hotels')
            .then(response => {
                this.hotels = response.data
                console.log(this.hotels)
            })
            .catch( error => { console.log(error) } );
        
    },
    mounted() {
        axios.get('http://fake-hotel-api.herokuapp.com/api/reviews?hotel_id=${id}')
            .then(response => {
                this.reviews = response.data
                console.log(this.reviews)
            })
            .catch( error => { console.log(error) } );
    },
    methods: {
        toggleButton() {
            this.show = !this.show;
            this.button.text = !this.show ? "Show Reviews" : "Hide Reviews";
        }
    }
};

</script>




<style>
</style>