<template lang="pug">
.column.is-paddingless.airport-select
  label.airport-select__label {{ label }}
  input.airport-select__input.airport-select__placeholder(type="text" placeholder="Find the city" @input="searchAirport(query)" v-model="query" @click="showDropdownItems")
  .dropdown-items(:class="{'show-dropdown-items': showItems}")
    .dropdown-item(v-for="airport in airports" :key="airport.icao" @click="setAirport(airport)") {{ airport.city }}
     span.badge.badge-primary {{ airport.name }} {{ airport.country }}
     .sm-line
</template>

<script>
import ClientService from "@/services/ClientService.js";

export default {
  props: {
    label: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      showItems: false,
      airports: [],
      query: ""
    };
  },
  created() {
    ClientService.getAirports()
      .then(response => {
        this.airports = response.data;
      })
      .catch(error => {
        console.log("There was an error:", error.response);
      });
  },
  methods: {
    showDropdownItems() {
      this.showItems = !this.showItems;
    },
    setAirport(airport) {
      this.query = airport.city;
      this.$emit("updatePropValue", this.query);
      this.showItems = false;
    },
    searchAirport() {
      this.airports = this.airports.filter(airport => {
        return (
          airport.city.toLowerCase().indexOf(this.query.toLowerCase()) > -1
        );
      });
    }
  }
};
</script>

<style lang="sass">
.dropdown-items
  position: absolute
  top: 47px
  left: 0px
  z-index: 99
  background: white
  width: 100%
  display: none

.show-dropdown-items
  display: block

.dropdown-item
  cursor: pointer

.badge-primary
  margin-left: 10px

.sm-line
  height: 1px
  background: #b8daff8c
  width: 100%
  margin-top: 5px

.airport-select
  margin-top: 10px
  position: relative

  &__label
    color: white
    font-size: 16px

  &__placeholder::placeholder
    color: #f7b944

  &__input
    background-color: #495057 !important
    color: #f7b944 !important
    border-inline-start: 0px !important
    border-block-end: 1px solid black !important
    border-block-start: 0px !important
    border-inline-end: 0px !important
    width: 100%
    font-size: 16px
</style>
