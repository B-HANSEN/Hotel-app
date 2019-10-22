<template>



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

    mounted() {
        axios.get('http://fake-hotel-api.herokuapp.com/api/reviews')
            .then(response => {
                this.reviews = response.data
                console.log(this.reviews)
            })
            .catch( error => { console.log(error) } );
    }
};

</script>

