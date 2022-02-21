<template>
	<div id="search">
	<v-row>
          <v-col cols="12">
            <v-text-field
				v-model="searchedLocation"
				outlined
				clearable
				@keyup.enter="getLocation"
				label="Enter the location"
				type="text"
            >
              <template v-slot:prepend>
                <v-tooltip
                  bottom
                >
                  <template v-slot:activator="{ on }">
                    <v-icon v-on="on">
                      mdi-help-circle-outline
                    </v-icon>
                  </template>
					<div class="subtitle-1">
						Enter any city into the search box to get a list of city matches.
						<br>
						<span class="subtitle-2">Displays top 10 nearest locations to you by default (must share location in browser).</span>
					</div>
                </v-tooltip>
              </template>
              <template v-slot:append>
				<v-icon
					@click="getUserLocation"
                    width="16"
                    height="16"
					style="margin-right: 10px;"
                    alt="">
					mdi-crosshairs
                </v-icon>
                <v-fade-transition leave-absolute>
                  <v-progress-circular
                    v-if="loadingCities"
                    size="24"
                    color="info"
                    indeterminate
                  ></v-progress-circular>
                  <v-icon
					@click="getLocation"
                    v-else
                    width="24"
                    height="24"
                    alt="">
					search
                  </v-icon>
                </v-fade-transition>
              </template>
            </v-text-field>
          </v-col>
        </v-row>
	</div>
</template>

<script>
import axios from "axios";

const cors = "https://cors-anywhere.herokuapp.com/"; // use cors-anywhere to fetch api data
const url = "https://www.metaweather.com/"; // origin api url

export default {
	name: "app-search",
	data() {
		return {
			searchedLocation: this.value,
			loadingCities: false,
			error: false
		}
	},
	async created() {
		const success = (position) => {
			const latitude  = position.coords.latitude;
			const longitude = position.coords.longitude;
			this.getLocation(`${latitude},${longitude}`)
		};

		const error = () => {
			this.loadingCities = false;
		};
		this.loadingCities = true;
		await navigator.geolocation.getCurrentPosition(success, error);
	},
	methods: {
		getLocation(latLong) {
			this.loadingCities = true;
			let query = null;
			if (typeof latLong === 'string') {
				query = `?lattlong=${latLong}`
			} else {
				query = `?query=${this.searchedLocation}`
			}
			axios
				.get(
					`${cors}${url}api/location/search/` +
						query
				)
				.then(res => {
					this.$emit('city-list-data', res.data);
					this.loadingCities = false;
				})
				.catch(error => {
					this.loadingCities = false;
					this.error = true;
					throw error
				});
		},
		async getUserLocation() {
			const success = (position) => {
				const latitude  = position.coords.latitude;
				const longitude = position.coords.longitude;
				this.getLocation(`${latitude},${longitude}`)
			};

			const error = () => {
				this.$emit('city-list-data', []);
				this.loadingCities = false;
			};
			this.loadingCities = true;
			await navigator.geolocation.getCurrentPosition(success, error);
		}
	}
};
</script>

<style lang="scss" scoped>

</style>
