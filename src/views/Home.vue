<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      Name:
      <input type="text" v-model="newName" />
      Address:
      <input type="text" v-model="newAdress" />
      <button v-on:click="createPlace">Add a Place</button>
    </div>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>{{ place.name }}</h2>
      <button v-on:click="showPlace(place)">More Info</button>
    </div>
    <!-- links show action. Dialog is used to display something -->
    <dialog id="place-details">
      <form method="dialog">
        <h2>Place Info</h2>
        <p>
          Name:
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Adress:
          <input type="text" v-model="currentPlace.adress" />
        </p>
        <button>Close</button>
        <button v-on:click="updatePlace">Update Place</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Suh Dude!",
      places: [],
      newAdress: "",
      newName: "",
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/api/places/").then((response) => {
        this.places = response.data;
        console.log("all places:", this.places);
      });
    },
    createPlace: function () {
      console.log("adding PLACE!");
      var params = {
        name: this.newName,
        place: this.newAdress,
      };
      axios
        .post("/api/places", params)
        .then((response) => {
          console.log("Successfully added place!", response.data);
          this.places.push(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      var params = {
        name: place.name,
        adress: place.adress,
      };
      axios.patch("/api/places/" + place.id, params).then((response) => {
        console.log("Success!", response.data);
      });
    },
  },
};
</script>
