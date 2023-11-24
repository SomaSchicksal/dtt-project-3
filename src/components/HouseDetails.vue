<template>
    <!-- Basic house component that will be rendered every time you want to see
    the details of one of the houses the application gets from the api call -->
    <div>
        <h1>house details</h1>
        <div v-if="house && house.length > 0">
            <House :house="house[0]"></House>
        </div>
        <div v-else>
            <h1>Loading...</h1>
        </div>
    </div>
</template>

<script>

import House from './House.vue';
export default {
    data() {
        return {
            house: [],
        }
    },
    //as soon as the component is created i make an api call with the house's id as parameter

    async created() {
        const houseId = this.$route.params.id;

        const ApiKey = "_0KfzMBqomADtsj7brkVaX1iwRhLgIQN";

        /*calling the api with the id of the house as a parameter allows me to get 
        its specific infos and then render them */


        const response = await fetch(`https://api.intern.d-tt.nl/api/houses/${houseId}`, {
            method: "GET",
            headers: {
                "X-Api-Key": ApiKey,
            }
        }).then((res) => res.json())
            .then((data) => {
                console.log(data);
                this.house = data;
            });



    },

    components: { House }
}
</script>