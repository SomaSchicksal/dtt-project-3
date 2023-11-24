<template>
    <!-- This component renders the list of houses liked by the user-->
    <div>
        <h1>List of Favorites</h1>
        <div v-if="$store.state.myFavoriteHouses && $store.state.myFavoriteHouses.length > 0" v-for="houseId in $store.state.myFavoriteHouses" :key="houseId">
            <router-link :to='{ name: "house-details", params: { id: houseId } }'>
                <img :src="getHouseById(houseId).image" alt="House Image">
            </router-link>
            <button @click="toggleLike(houseId)">Like</button>
            <div>
                <h1>city:{{ getHouseById(houseId).location ? getHouseById(houseId).location.city : '' }}</h1>
                <h1>street:{{ getHouseById(houseId).location ? getHouseById(houseId).location.street : '' }}</h1>
                <h1>number:{{ getHouseById(houseId).location ? getHouseById(houseId).location.houseNumber : '' }}</h1>
                <h1>Price: {{ getHouseById(houseId).price }}</h1>
                <h1>size:{{ getHouseById(houseId).size }}</h1>
                <h1>bed:{{ getHouseById(houseId).rooms ? getHouseById(houseId).rooms.bedrooms : '' }}</h1>
                <h1>bath:{{ getHouseById(houseId).rooms ? getHouseById(houseId).rooms.bathrooms : '' }}</h1>
            </div>
        </div>
        <div v-else>
            <h1>Loading...</h1>
        </div>
    </div>
</template>

<script>
import House from "../components/House.vue"

export default {
    methods: {
        toggleLike(houseId) {
            // Logic to remove a house if the user presses the like button again
            this.$store.dispatch('toggleLike', houseId);
            console.log('Toggling like for houseId:', houseId);
        },
        //logic that gets a specific house from the api based on its id
        getHouseById(id) {
            return this.$store.state.apiData.find(house => house.id === id);
        },
    },
    components: { House }
}
</script>