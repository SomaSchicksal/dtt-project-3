<template>
  <!-- This component renders the homepage, as soon as it's loaded a list of houses will
  gotten from the api will be rendered. The user can filter them by price or size, or look
for a specific house by typing in the searchbar. Here the user can even create a house-->
  <router-link to="/houseForm"><button>create</button></router-link>
  <!-- typing in the searchbar will fire the function that handle the filtering of the houses -->
  <input v-model="search" placeholder="search for a house" @input="filterHouses" />
  <span v-if="search !== ''" @click="clearSearchInput" class="clear-button">&#10006;</span>
  <!-- These buttons fire the function that handle the filtering by price and size -->
  <button @click="sortByPrice">Price</button>
  <button @click="sortBySize">Size</button>
  <!-- a div will indicate the results of the search if the user uses the searchbar -->
  <div class="result-indication" v-if="filteringOccurred">
    <p v-if="filteredHouses.length > 0">
      {{ filteredHouses.length }} result{{ filteredHouses.length !== 1 ? 's' : '' }} found.
    </p>
    <p v-else>No results found.</p>
  </div>

  <!-- simple for loop to render all houses i get from the api call, for each house i get 
  i render a house component that will render its details -->
  <div class="home" v-for="house in houses" :key="house.id">

    <House :house="house"></House>



  </div>
</template>

<script>


import House from '@/components/House.vue';


export default {
  data() {
    return {
      houses: [],
      search: "",
      filteredHouses: [],
      filteringOccurred: false

    }
  },
  name: 'HomeView',
  components: {

  },
  methods: {
    //function that makes the api call
    getHouses() {
      this.houses = [];
      this.$store.dispatch("fetchApiData").then(() => {
        this.houses = this.$store.state.apiData;


      })
    },

    //filtering functions

    sortByPrice() {
      this.houses.sort((a, b) => a.price - b.price);
    },

    sortBySize() {
      this.houses.sort((a, b) => a.size - b.size);
    },

    //logic that handles the filtering of the search bar



    filterHouses() {

      const searchTerm = this.search.toLowerCase();
      this.houses = this.$store.state.apiData.filter((house) => {
        return house.location.city.toLowerCase().includes(searchTerm)
          || house.location.zip.toLowerCase().includes(searchTerm)
          || house.size.toString().includes(searchTerm)
          || house.price.toString().includes(searchTerm);
      });
      this.filteredHouses = this.houses;
      this.filteringOccurred = true;


    },

    clearSearchInput() {
      this.search = '';
      this.getHouses()

    }
  },

  computed: {

    //function that handles the filtering of the searchbar
    filteredHouses() {
      const searchTerm = this.search.toLowerCase();
      return this.houses.filter((house) => {
        const cityMatch = house.location.city.toLowerCase().includes(searchTerm);
        const zipMatch = house.location.zip.toLowerCase().includes(searchTerm);


        const sizeMatch = house.size.toString().includes(searchTerm);
        const priceMatch = house.price.toString().includes(searchTerm);

        return cityMatch || zipMatch || sizeMatch || priceMatch;
      })
    }
  },



  mounted() {

    // i make the api call as soon as the component is mounted with this function
    this.getHouses();
    console.log(this.houses);


  },

  components: { House }

}
</script>

<style scoped>
/* Style for the clear button */
.clear-button {
  cursor: pointer;
  color: #999;
  /* Adjust the color as needed */
  margin-left: 5px;
  /* Adjust the spacing as needed */
}
</style>