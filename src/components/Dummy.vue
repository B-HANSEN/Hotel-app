<template>

  <!-- hotel container -->

  <v-container>
            <v-layout class="hotelContainer">
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
                    <v-responsive
                    :aspect-ratio="4/3" 
                    >
                        <v-card class="action">
                            <v-img 
                            :src="hotel.images[0]"
                            ></v-img>
                        </v-card>
                     </v-responsive>
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








         <div id="root">

        <div class="my-2">
            <v-btn small>Load Hotels</v-btn>
        </div>

        <v-container>
        <!-- optional paragraph in case no output -->
        <div>
            <p v-if="!hotels.length">An error occured.</p>
        </div>

        <v-flex  
            v-for="(hotel,idHotel) in hotels"
            :key="idHotel"
        >
        
            <v-card
                class="d-inline-block mx-auto"
                max-width="600"
            >
                <div>
                <div class="d-inline">
                    <v-img
                    :src="hotel.images[0]"
                    height="600px"
                    ></v-img>
                </div>

                <div class="d-inline">
                    <v-card-title>
                    <h2>{{ hotel.name }}</h2>
                    </v-card-title>

                    <v-card-subtitle>
                    <h3> {{ hotel.city }} - {{ hotel.country}}</h3>
                    </v-card-subtitle>

                    <p>{{ hotel.description }}</p>
                    <p>{{ hotel.price }} €</p>

                    <!-- button to show more -->
                    <v-card-actions>
                        <v-btn
                            icon
                            @click="show = !show"
                        >
                            <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
                        </v-btn>
                    </v-card-actions>
                </div>
                </div>

                    <v-expand-transition>
                    <!-- <div v-show="show"> -->
                        <v-divider></v-divider>

                        <v-card-text>
                        I'm a thing. But, like most politicians, he promised more than he could deliver. You won't have time for sleeping, soldier, not with all the bed making you'll be doing. Then we'll go with that data file! Hey, you add a one and two zeros to that or we walk! You're going to do his laundry? I've got to find a way to escape.
                        </v-card-text>
                    <!-- </div> -->
                    </v-expand-transition>
        
            </v-card>

        </v-flex>
        </v-container>

    </div>

</template>