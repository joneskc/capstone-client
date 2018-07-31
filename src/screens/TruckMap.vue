<template>
  <view class="container">
    <nb-container :style="{backgroundColor: '#fff'}">
      <nb-header>
        <nb-left>
          <nb-button
            transparent
            :onPress="() => this.props.navigation.goBack()"
          >
            <nb-icon name="arrow-back" />
          </nb-button>
        </nb-left>
        <nb-body>
          <image
            :style="{ width: 33, height: 30 }"
            :source="smallLogo"
          />
        </nb-body>
        <nb-right>
          <nb-button danger small :onPress="logoutTruck">
            <nb-text>Close</nb-text>
          </nb-button>
        </nb-right>
      </nb-header>

      <nb-container>
        <map-view
          :style="{ height: '70%' }"
          :showsUserLocation="true"
          :loadingEnabled="true"
          :minZoomLevel=0
          :maxZoomLevel=20
          :initial-region="coordinates">
          <Marker
            v-if="truck.open"
            v-for="truck in trucks"
            :key="truck.name"
            :truck="truck"
            :coordinate="{
              latitude: truck.latitude,
              longitude: truck.longitude,
              latitudeDelta: 0.0922,
              longitudeDelta: 0.0421
            }"
            :title="truck.name"
            :description="truck.category"
          />
        </map-view>
        <nb-content :style="{ height: '25%' }">
          <scrollView scrollEnabled :style="{ flex: 1 }">
            <nb-list>
              <nb-list-item
                v-if="truck.open"
                v-for="truck in trucks"
                :key="truck.name">
                <nb-left>
                  <nb-text>{{ truck.name }}</nb-text>
                </nb-left>
                <nb-right>
                  <nb-icon name="arrow-forward" />
                </nb-right>
              </nb-list-item>
            </nb-list>
          </scrollView>
          </nb-content>
      </nb-container>
    </nb-container>
  </view>
</template>

<script>
import { Constants, MapView, Location, Permissions } from 'expo';
import { Marker } from 'react-native-maps';
import smallLogo from '../assets/smallLogo.png';

export default {
  components: {
   MapView,
   Marker
  },
  props: {
    navigation: {
      type: Object
    },
    location: {},
    errorMessage: '',
    truckPins: null,
    truckUsername: ''
  },
  data: function() {
    return {
      smallLogo: smallLogo,
      ListTab: false,
      MapTab: true,
      ProfileTab: false,
      trucks: [],
      truck: null,
      targetedTruck: '',
      updatedTruck: {
        open: false
      },
      // truckCoordinates: {
      // //   // latitude: location.coords.latitude,
      // //   // longitude: location.coords.longitude,
      // //   // latitude: 39.7587,
      // //   // longitude: -105.008,
      //   latitude: null,
      //   longitude: null,
      //   latitudeDelta: 0.0922,
      //   longitudeDelta: 0.0421
      // },
      coordinates: {
        latitude: 39.759109,
        longitude: -105.004018,
        // the actual coordinates below are from actual device geolocation, this didnt work on desktop for demo, it defaulted to San Francisco, so I had to hard code coordinates for demo
        // latitude: location.coords.latitude,
        // longitude: location.coords.longitude,
        latitudeDelta: 0.0922,
        longitudeDelta: 0.0421
      }
    }
  },
  methods: {
    logoutTruck: function() {
        const UPDATE_TRUCK_API_URL = `https://aqueous-forest-61110.herokuapp.com/api/trucks/${this.targetedTruck}`
        const data = this.updatedTruck
        fetch(UPDATE_TRUCK_API_URL, {
            method: "PUT",
            headers: {"content-type": "application/json"},
            body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
            this.navigation.navigate("MainScreen")
        })
    },
  },
  beforeMount() {
    this.trucks = this.navigation.getParam("truckPins")
    this.targetedTruck = this.navigation.getParam("truckUsername")
    console.log("my trucks", this.trucks)
    console.log("truck user name", this.targetedTruck)
  }
};
</script>

<style>
.container {
  flex: 1;
  height: 60%;
}
.text-color-primary {
  color: blue;
}
</style>
