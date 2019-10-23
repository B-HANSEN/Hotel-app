<template>
    <div>

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
            reviews: [],
            errors: []
        }
    },

    // fetch REVIEWS from remote
    // TODO: verify $id-logic
    mounted() {
        axios.get('http://fake-hotel-api.herokuapp.com/api/reviews?hotel_id=$"id"')
            .then(response => {
                this.reviews = response.data
                console.log(this.reviews)
            })
            .catch( error => { console.log(error) } );
    }
};

</script>