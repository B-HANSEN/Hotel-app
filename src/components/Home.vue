<template>
     <div class="d-flex flex-column">

<!-- ================ LOAD HOTELS BUTTON ============= -->
        <div class="text-center my-5">
            <v-btn @click="this.handleLoad" outlined text>Load Hotels</v-btn>
        </div>

        <v-container v-if="showData">
        <v-flex  
            v-for="(hotel,id) in hotels"
            :key="id"
        >
<!-- ========================== HOTEL CONTAINER ========================== -->
            <v-card
                class="mx-auto mb-2 grey lighten-2"
                max-width="95%"
                outlined
            >
                <div class="d-flex flex-row">
                    <v-container fluid class="photocontainer">
                    <v-img
                        :src="hotel.images[0]"
                        :aspect-ratio="1.3333"
                        max-width="266px"
                        max-height="200px"
                    ></v-img>
                    </v-container>

                    <div class="d-flex flex-column">
                        <div class="d-flex flex-row justify-space-between">
                            <div class="title pl-3">{{ hotel.name }}</div>

<!-- ============== STAR RATING SECTION ============== -->
                            <div class="stars">
                                <div 
                                    v-for="index in 5"
                                    :key="index"
                                >
                                    <div v-if = "index <= hotel.stars">
                                        <i class="fas fa-star"></i>
                                    </div>

                                    <div v-else>
                                        <i class="far fa-star"></i>
                                    </div>
                                
                                </div>
                            </div>
                        </div> 
                        <div class="subtitle-2 pl-3">{{ hotel.city }} - {{ hotel.country }}</div>
                    
                        <v-list-item three-line class="description">{{ hotel.description }}
                        </v-list-item>
                 
<!-- ================= REVIEW BUTTON ================= -->
                        <div class="d-flex flex-row justify-space-between align-center px-3">
                            <v-card-actions>
                                <v-btn v-if="show == hotel.id"
                                    @click="handleReview(hotel.id)">Hide reviews
                                </v-btn>
                                    <v-btn v-else
                                    @click="handleReview(hotel.id)">Show reviews
                                </v-btn>
                            </v-card-actions>
                        
<!-- =========== PRICE & TIME RANGE SECTION ========== -->
                            <div class="pricetime">
                                <h4 class="align-self-end">{{ hotel.price }} €</h4>
                                <span>{{ [ hotel.date_start ] | moment("DD.MM.YYYY") }} - {{ [ hotel.date_end ] | moment("DD.MM.YYYY") }}</span>
                            </div>
                        </div>
                    </div>
                </div>

<!-- ========================= REVIEW CONTAINER ========================== -->
                 <v-card class="grey lighten-1 py-4" v-if="show == hotel.id">
                    <v-flex
                        v-for="review in reviews"
                        :key="review.hotel_id"
                    >
                        <v-card
                            class="grey lighten-1"
                            max-width="100%"
                            outlined
                        >
                            <v-expand-transition >
                                <v-show="show">

                                    <div class="d-flex flex-row">
                                        <i class="material-icons positive align-self-center mx-4 grey lighten-2"
                                            v-if="review.positive == true">add
                                        </i>
                                        <i class="material-icons positive align-self-center mx-4 grey lighten-2"
                                            v-else>remove
                                        </i>

                                        <div>
                                            <h6>{{ review.name }}</h6>
                                            <p class="comments">{{ review.comment }}</p>
                                        </div>
                                    </div>
                            </v-expand-transition>
                        </v-card>
                    </v-flex>
                </v-card>
            </v-card>
        </v-flex>
        </v-container>           

<!-- ========================== ERROR CONTAINER ========================== -->
        <v-container v-if="errorMsg">
                <h5 class="errorfield">An error occured</h5>
        </v-container>

<!-- =============== BACK TO TOP BUTTON ============== -->
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
            show: "",
            showData: false,
            errorMsg: false
        }
    },
    components: { BackToTop },

    methods: {
        async handleLoad() {
            try {
                const response = await axios.get('http://fake-hotel-api.herokuapp.com/api/hotels?count=5')
                this.hotels = response.data;
                this.showData = true
                this.errorMsg = false
            return this.hotels;
            } catch (error) {
                this.showData = false
                this.errorMsg = true
            }
        },

        async handleReview(id) {
            this.show == id ? this.show = "" : this.show = id;
            if(this.show.length !=0)
            try {
                const response = await axios.get(`http://fake-hotel-api.herokuapp.com/api/reviews?hotel_id=${id}`)
                this.reviews = response.data;
            return this.reviews;                
            } catch (error) {
               console.error(error);
            }
        }
    }
}

</script>

<style>
.photocontainer {
    height: "200px";
    padding: 0px !important;
}

.container--fluid {
    max-width: 35% !important;
    margin: 0;
}

.description {
    font-size: 11px;
}

.errorfield {
    width: 95%;
    margin: auto;
    color: #5d5757;
    background-color: #c7c7c7;
    padding: 5px;
    border: 1px solid darkgrey;
}

.pricetime {
    display: flex;
    flex-direction: column;
    justify-content: center
}

.pricetime >span {
    font-size: 10px;
}

.btn-to-top {
  width: 40px;
  height: 40px;
  border: 1px solid lightgrey;
  border-radius: 50%;
  font-size: 22px;
  line-height: 22px;
}

.stars {
    display: flex;
    margin-right: 20px;
    margin-top: 10px;
}

.positive {
    size: 20px;
    border: 2px solid black;
    border-radius: 50%;
    margin-bottom: 14px;
}

.comments {
    font-size: 10px;
}

</style>