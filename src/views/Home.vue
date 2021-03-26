<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>Place: <button</div>
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
      newPlaceAdress: "",
      newPlaceName: "",
    };
  },
  created: function () {},
  methods: {},
};
</script>
