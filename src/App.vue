<template>
    <app-navigation
      :location="location"
      :errorMessage="errorMessage"
      :getLocation="getLocation()"
    ></app-navigation>
</template>

<script>
import Vue from "vue-native-core";
import { VueNativeBase, Root } from "native-base";
import { StackNavigator } from "vue-native-router";
import { Permissions, Constants, Location } from "expo";
import MainScreen from './screens/Main';
import TruckMapScreen from './screens/TruckMap';
import TruckLoginScreen from './screens/TruckLogin';
import UserMapScreen from './screens/UserMap';

Vue.use(VueNativeBase);

const AppNavigation = StackNavigator(
  {
    MainScreen: { screen: MainScreen },
    TruckMapScreen: { screen: TruckMapScreen },
    TruckLoginScreen: { screen: TruckLoginScreen },
    UserMapScreen: { screen: UserMapScreen }
  },
  {
    initialRouteName: 'MainScreen',
    headerMode: 'none'
  }
);
export default {
    components: { Root, AppNavigation },
    data: function() {
      return {
        location: {},
        errorMessage: "",
        trucks: []
      }
    },
    methods: {
      getLocation: function() {
        Permissions.askAsync(Permissions.LOCATION).then(status => {
          if (status !== "granted") {
            errorMessage = "Permission to access location was denied";
          }
          Location.getCurrentPositionAsync({}).then(location1 => {
            location = location1;
          });
          }).catch((err)=>{
            console.log(err);
        });
      }
    }
}
</script>

<style>

</style>
