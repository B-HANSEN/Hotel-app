<template>
   <div class="d-flex flex-column">

        <v-btn text mb-5>Load Hotels</v-btn>

        <v-flex  
            v-for="(hotel,idHotel) in hotels"
            :key="idHotel"
        >

<!-- ==================== HOTEL CONTAINER ====================-->
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
                                <v-btn v-if="!show" @click="toggleButton" text>Show Reviews</v-btn>
                            </v-card-actions>

                            <h4 class="align-center ">{{ hotel.price }} €</h4>
                            <!-- <p class="align-center ">{{ hotel.date_start }} - {{ hotel.date_end }}</p> -->
                        </div>
                    </div>
                </div>
            </v-card>
        </v-flex>

<!-- ==================== REVIEW CONTAINER ====================-->

        <v-flex  
            v-for="(review,idHotel) in reviews"
            :key="idHotel"
        >

            <v-card
                class="mx-auto"
                max-width="800"
                outlined
            >

                <div  class="d-flex flex-row">
                    <div>
                        <p>{{ review.positive }}
                        </p>
                    </div>

                    <div>
                        <h2>{{ review.name }}
                        </h2>

                        <h4>{{ review.comment }}
                        </h4>
                    </div>
                </div>
            </v-card>
        </v-flex>
                   
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
        }
    },

    mounted() {
        axios.get('http://fake-hotel-api.herokuapp.com/api/hotels')
            .then(response => {
                this.hotels = response.data
                console.log(this.hotels)
            })
            .catch( error => { console.log(error) } );
        
        axios.get('http://fake-hotel-api.herokuapp.com/api/reviews?hotel_id=${id}')
                .then(response => {
                    this.reviews = response.data
                    console.log(this.reviews)
                })
                .catch( error => { console.log(error) } );
    }
};

</script>




<style>
</style>