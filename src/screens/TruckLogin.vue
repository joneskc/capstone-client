<template>
    <nb-container>
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
          <nb-right />
        </nb-header>
        <nb-content>
            <nb-form>
                <nb-item floatingLabel>
                    <nb-label>Username</nb-label>
                    <nb-input v-model="userInput" />
                </nb-item>
                <nb-button :style="stylesObj.btnContainer" :onPress="updateTruck" :location="location" :allTrucks="allTrucks">
                    <nb-text>We're Open!</nb-text>
                </nb-button>
                <nb-button :style="stylesObj.btnContainer" :onPress="updateTruck" :location="location" :allTrucks="allTrucks">
                    <nb-text>New Truck</nb-text>
                </nb-button>
            </nb-form>
        </nb-content>
    </nb-container>
</template>

<script>
import smallLogo from '../assets/smallLogo.png';

export default {
props: {
    navigation: {
        type: Object
    },
    location: {}
},
data: function () {
    return {
        allTrucks: {},
        truckPins: null,
        userInput: '',
        updatedTruck: {
            latitude: location.coords.latitude,
            longitude: location.coords.longitude,
            open: true
        },
        smallLogo: smallLogo,
        stylesObj: {
            btnContainer: {
            backgroundColor: "#6faf98",
            alignSelf: "center",
            marginTop: 10
            }
        }
    }
},
methods: {
    updateTruck: function() {
        const UPDATE_TRUCK_API_URL = `https://aqueous-forest-61110.herokuapp.com/api/trucks/${this.userInput}`
        const data = this.updatedTruck
        fetch(UPDATE_TRUCK_API_URL, {
            method: "PUT",
            headers: {"content-type": "application/json"},
            body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
            this.getTrucks()
        })
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
                this.truckPins = this.allTrucks.data
            })
            .then(data => {
                this.toTruckMap()
            })
    },
    toTruckMap: function() {
        this.navigation.navigate("TruckMapScreen", {
            truckPins:this.truckPins,
            truckUsername:this.userInput
        })
    },
}
}
</script>
