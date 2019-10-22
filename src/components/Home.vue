<template>
   <div>

            <v-flex  
                    v-for="(hotel,idHotel) in hotels"
                    :key="idHotel"
                >

            <v-card class="d-inline-block mx-auto">
            <v-container>
            <v-row justify="space-between">
                <v-col cols="auto">
                <v-img
                    height="600"
                    width="600"
                    :src="hotel.images[0]"
                ></v-img>
                </v-col>

                <v-col
                cols="auto"
                class="text-center pl-0"
                >
                <v-row
                    class="flex-column ma-0 fill-height"
                    justify="center"
                >
                    <v-col class="px-0">
                        <h2>{{ hotel.name }}</h2>
                    </v-col>

                    <v-col class="px-0">
                        <h3> {{ hotel.city }} - {{ hotel.country}}</h3>
                    </v-col>

                    <v-col class="px-0">
                        <p>{{ hotel.description }}</p>
                        <p>{{ hotel.price }} €</p>
                    </v-col>
                </v-row>
                </v-col>
            </v-row>
            </v-container>
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