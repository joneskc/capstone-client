<template>
  <nb-container>
    <nb-content padder :style="{ marginTop: 170 }">
      <view class='image-container'>
        <image
          :style="{ width: 300, height: 195 }"
          :source="logo"
        />
      </view>
      <nb-button :style="stylesObj.btnContainer" :onPress="getTrucks" :location="location">
        <nb-text>Go to Map</nb-text>
      </nb-button>
      <nb-button :style="stylesObj.btnContainer" :onPress="toLogin" :location="location">
        <nb-text>Truck Login</nb-text>
      </nb-button>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform } from "react-native";
import { Permissions, Constants, Location } from "expo";
import logo from '../assets/logo.png';

export default {
  props: {
    navigation: {
      type: Object
    },
    location: {},
    errorMessage: "",
    getLocation: Function
  },
  data: function() {
    return {
      logo: logo,
      truckPins: [],
      stylesObj: {
        logoContainerStyle: {
          marginTop: Dimensions.get("window").height / 8
        },
        btnContainer: {
          backgroundColor: "#6faf98",
          alignSelf: "center",
          marginTop: 10
        }
      }
    };
  },
  methods: {
    toLogin: function() {
      this.navigation.navigate("TruckLoginScreen")
    },
    getTrucks: function() {
      const TRUCKS_API_URL = 'https://aqueous-forest-61110.herokuapp.com/api/trucks'
      fetch(TRUCKS_API_URL)
        .then(res => res.json())
        .then(res => {
          this.allTrucks = res
          console.log(this.allTrucks.data)
        })
        .then(data => {
          this.truckPins = this.allTrucks.data.filter(truck => truck.open === true)
        })
        .then(data => {
            this.toUserMap()
        })
    },
    toUserMap: function() {
      this.navigation.navigate("UserMapScreen", {
        truckPins:this.truckPins,
        truckUsername:this.userInput
      })
    }
  }
}
</script>

<style>
.text-color-primary {
  color: blue;
  font-family: Roboto;
}
.text-container {
  align-items: center;
  margin-bottom: 50;
  background-color: transparent;
}
.text-color-white {
  color: white;
}
.button-container {
  background-color: #6faf98;
  align-self: center;
}
.image-container {
  position: relative;
  align-items: center;
}
</style>

