<template>
    <!-- Basic house component that will be rendered every time you see 
        an house listing in the application -->
    <div v-if="house" class="houses">
        <!-- If the madeByMe property is true im gonna rendere the data of the houses im 
            creating, otherwise ill just render the data of the houses from the api-->

        <div v-if="house && house.madeByMe">
            <h1>{{ house.city }}</h1>
            <h1>{{ house.street }}</h1>
            <img :src="imageUrl ? imageUrl : (house.image ? house.image : 'default-image-url')" alt="House Image">
            <h1>{{ house.number }}</h1>
            <h1>{{ house.addition }}</h1>
            <h1>{{ house.postalCode }}</h1>
            <h1>{{ house.price }}</h1>
            <h1>{{ house.size }}</h1>
            <h1>{{ house.garage }}</h1>
            <h1>{{ house.bedrooms }}</h1>
            <h1>{{ house.bathrooms }}</h1>
            <h1>{{ house.constructionDate }}</h1>
            <h1>{{ house.details }}</h1>

        </div>

        <div v-else class="api-houses">
            <div v-if="house" class="api-house">
                <router-link :to='{ name: "house-details", params: { id: house.id } }'>
                    <img :src="house.image" class="house-image">
                </router-link>

                <!-- this button fires the toggle like function that handles the functionality
            to store the liked houses and render them in the favorites section -->

                <div class="house-info">

                    <div class="house-address">
                        <h1>{{ house.location ? house.location.street : '' }}</h1>
                        <h1>{{ house.location ? house.location.houseNumber : '' }}</h1>
                    </div>
                    <div class="price">
                        <img :src="price">
                        <h1>{{ house.price }}</h1>
                    </div>
                    <div class="zip-city">
                        <h1>{{ house.location ? house.location.zip : '' }}</h1>
                        <h1>{{ house.location ? house.location.city : '' }}</h1>
                    </div>
                    <div class="rooms">
                        
                        <div class="bed">
                            <img :src="bed">
                            <h1>{{ house.rooms ? house.rooms.bedrooms : '' }}</h1>
                        </div>
                        <div class="bath">
                            <img :src="bath">
                            <h1>{{ house.rooms ? house.rooms.bathrooms : ''  }}</h1>
                        </div>
                        <div class="size">
                            <img :src="size">
                            <h1>{{ house.size }}</h1>
                        </div>
                        
                    </div>
                </div>
                <button @click="toggleLike">Like</button>
            </div>
        </div>
    </div>
</template>

<script>

import FavoritesList from '@/views/FavoritesList.vue';
import Bath from "@/assets/ic_bath@3x.png"
import Bed from "@/assets/ic_bed@3x.png"
import Size from "@/assets/ic_size@3x.png"
import Price from "@/assets/ic_price@3x.png"

export default {
    data() {
        return {

            bath:Bath,
            bed:Bed,
            size:Size,
            price:Price,

        }
    },
    // these are the props that im going to get from the parent components, 
    //i just need an house object and a url for the image
    props: {
        house: {
            type: Object,
            required: true,
        },

        imageUrl: {
            type: String,
            default: "", // Set a default value or leave it as an empty string
        },

    },

    computed: {
        liked() {
            return this.$store.state.myFavoriteHouses.includes(this.house.id);
        },
    },

    methods: {
        toggleLike() {
            this.$store.dispatch('toggleLike', this.house.id);
            console.log("clicked")
        },
    },

    mounted() {
        // Logs the received props to the console
        console.log('Received props in House component:', this.house);
    },

    components: { FavoritesList }




}

</script>

<style scoped>


.api-houses {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 20px;
    
    
    
    
}


.house-image {
    width: 250px;
}

.api-house {
    display: flex;
    width: 300px;
    gap: 20px;
}

.house-info {
    display: flex;
    flex-direction: column;
    
}

.house-address {
    display: flex;
}

.zip-city {
    display:flex;
    gap:6px;
}

.rooms {
    display:flex;
}

.price, .bed, .size, .bath {
    display:flex;
    
    
}








</style>