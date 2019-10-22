<template>
    <div id="root">

        <div class="my-2">
            <v-btn small>Load Hotels</v-btn>
        </div>

        <v-container>
            <!-- optional paragraph in case no output -->
            <div>
                <p v-if="!hotels.length">An error occured.</p>
            </div>

            <!-- hotel container -->
            <v-layout class="justify-center">
            <v-flex
                class="pb-2 input-parent"
                xs10
                offset-xs1
                v-for="(hotel,idHotel) in hotels"
                :key="idHotel"
            >
                <!-- photo container -->
                <v-layout align-center>
                <v-flex class="card">
                    <v-card class="action">
                        <v-img :src="hotel.images[0]"></v-img>
                    </v-card>
                </v-flex>
                </v-layout>

                <!-- info container -->
                 <v-layout align-center>
                <v-flex class="card">
                    <v-card height="100%" class="action">
                    <v-layout column pl-3 pt-1>
                        <v-flex>
                            <h2>{{ hotel.name }}</h2>
                            <h3>{{ hotel.city }} - {{ hotel.country}}</h3>
                            <p>{{ hotel.description }}</p>
                            <p>{{ hotel.price }} €</p>
                        </v-flex>
                    </v-layout>
                    </v-card>
                </v-flex>
                </v-layout>
            </v-flex>
            </v-layout>
        </v-container>


      


    </div>
</template>





<script>
import axios from 'axios';

export default {
    data() {
        return {
            hotels: [],
            errors: []
        }
    },

    mounted() {
        axios.get('http://fake-hotel-api.herokuapp.com/api/hotels')
            .then(response => {
                this.hotels = response.data
                console.log(this.hotels)
            })
            .catch( error => { console.log(error) } );
    }
};

</script>










<style>

</style>