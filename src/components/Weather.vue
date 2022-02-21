<template>
	<div id="weather" class="col-md-10 offset-md-1 text-center">
		<h1>5 Day Weather Forecast</h1>
		<search @city-list-data="setCityListData" />
		<v-row v-if="!loadingCities">
			<v-expansion-panels>
				<v-expansion-panel
					@click="details(weatherCity.woeid)"
					v-for="weatherCity in weatherCityList"
					:key="weatherCity.woeid"
					class="results__panel"
				>
					<v-expansion-panel-header class="results__header">
						<span class="header">{{ weatherCity.title  }}</span>
						<template v-slot:actions>
							<v-icon color="white" large>mdi-chevron-down</v-icon>
						</template>
					</v-expansion-panel-header>
					<v-expansion-panel-content v-if="loading">
						<div class="loading_box">
							<p>Loading...</p>
							<div class="line"></div>
							<div class="line"></div>
							<div class="line"></div>
						</div>
					</v-expansion-panel-content>
					<v-expansion-panel-content v-else-if="!loading">
						<v-row cols="11">
							<v-col
								cols="2.5"
								class="details__block"
								v-for="weatherData in weatherDataList"
								:key="weatherData.id"
							>
								<h3 class="details__date">
									{{ momentWeekday(weatherData.applicable_date) }}
								</h3>
								<h3 class="details__date">
									{{ momentDate(weatherData.applicable_date) }}
								</h3>
								<img
									:src="
										`https://www.metaweather.com/static/img/weather/${weatherData.weather_state_abbr}.svg`
									"
								/>
								<p class="details__text">
									<span class="details__label">Max:</span>
									{{ Math.round(weatherData.min_temp) }}°C
								</p>
								<p class="details__text">
									<span class="details__label">Min:</span>
									{{ Math.round(weatherData.max_temp) }}°C
								</p>
								<p class="details__text">
									<span class="details__label">Wind:</span>
									{{ Math.round(weatherData.wind_speed) }}mph {{weatherData.wind_direction_compass}}
								</p>
								<p class="details__text">
									<span class="details__label">Air Pressure:</span>
									{{ Math.round(weatherData.air_pressure) }}atm
								</p>
								<p class="details__text">💧: {{ weatherData.humidity }}%</p>
							</v-col>
						</v-row>
						<v-row class="justify-center">
							<div class="chart-block">
								<bar-chart
									v-if="!loading"
									:chartdata="maxTempData"
									:chartlabels="chartlabels"
									label="Max Temp (°C)"
									title="Max Temperature (°C)"
									color="#C88B32"
								/>
							</div>
							<div class="chart-block">
								<bar-chart
									v-if="!loading"
									:chartdata="minTempData"
									:chartlabels="chartlabels"
									label="Min Temp (°C)"
									title="Min Temperature(°C)"
									color="#01697D"
								/>
							</div>
							<div class="chart-block">
								<line-chart
									v-if="!loading"
									:chartdata="humidityData"
									:chartlabels="chartlabels"
									title="Humidity (%)"
								/>
							</div>
						</v-row>
					</v-expansion-panel-content>
				</v-expansion-panel>
			</v-expansion-panels>
		</v-row>
		<div v-if="weatherCityList && !weatherCityList.length">
			<v-alert 
				:value="alert && alert.info !== ''"
				icon="warning"
				transition="slide-x-transition"
				color="blue"
				type="info">
					{{ alert.info }}
			</v-alert>
		</div>
	</div>
</template>

<script>
import axios from "axios";
import BarChart from "@/components/widgets/BarChart";
import LineChart from "@/components/widgets/LineChart";
import Search from "@/components/widgets/Search";
import moment from "moment";

const cors = "https://cors-anywhere.herokuapp.com/"; // use cors-anywhere to fetch api data
const url = "https://www.metaweather.com/"; // origin api url

export default {
	name: "app-weather",
	components: {
		Search,
		BarChart,
		LineChart
	},
	data() {
		return {
			weatherCityList: null,
			weatherDataList: [],
			loading: true,
			alert: {
				info: '',
				error: ''
			},
			loadingCities: false,
			maxTempData: null,
			minTempData: null,
			humidityData: null,
			windData: null,
			airPressure: null,
			chartlabels: [],
		};
	},
	mounted() {
		if (alert) {
			this.hideAlert();
		}
	},
	methods: {
		setCityListData(data) {
			this.weatherCityList = data;
			this.loadingCities = false;
			if (!data.length) {
				this.alert.info = 'No data is available for that location.';
			}
		},
		details(woeId) {
			this.loading = true;
			axios
				.get(`${cors}${url}/api/location/` + woeId)
				.then(res => {
					res.data.consolidated_weather.pop();
					this.weatherDataList = res.data.consolidated_weather;
					this.maxTempData = this.weatherDataList.map(
						e => `${Math.round(e.max_temp)}`
					);
					this.minTempData = this.weatherDataList.map(
						e => `${Math.round(e.min_temp)}`
					);
					this.humidityData = this.weatherDataList.map(
						e => `${e.humidity}`
					);
					this.chartlabels = this.weatherDataList.map(
						e => `${e.applicable_date}`
					);
					this.loading = false;
				})
				.catch(error => {
					console.log(error);
				});
		},
		momentWeekday(date) {
			const day = moment(date).weekday();
			if(day === 0) {
				return 'Sunday';
			} else if (day === 1) {
				return 'Monday';
			} else if (day === 2) {
				return 'Tuesday';
			} else if (day === 3) {
				return 'Wednesday';
			} else if (day === 4) {
				return 'Thursday';
			} else if (day === 5) {
				return 'Friday';
			} else if (day === 6) {
				return 'Saturday';
			}
		},
		momentDate(date) {
			return moment(date).format('MM/DD');
		},
		hideAlert() {
			window.setInterval(() => {
				this.alert.info = '';
				this.alert.error = '';
			}, 5000)    
		}
	}
};
</script>
<style lang="scss" scoped>
#weather {
	h1 {
		color: #494c4d;
	}
}
input {
	width: 50%;
	height: 20px;
	border: 2px solid #23a6b8;
}
button {
	margin: 0 10px;
}
.v-expansion-panel.results__panel {
    margin-top: 10px;
}
.results {
	&__panel {
		background-color: #314e50 !important;
	}
	&__header {
		font-size: 24px; 
		color: #f8fafc;
	}
}
.details {
	&__block {
		margin: 10px 0;
	}
	&__date {
		color: #cbcdd0;
	}
	&__label {
		font-weight: bold;
		color: #cbcdd0;
	}
	&__text {
		margin-bottom: 5px;
		color:#cbcdd0;
	}
}
.chart-block {
	margin: 10px;
	display: inline-block;
}
.loading_box {
	margin: 20px;
	p {
		font-weight: bold;
		color: #cbcdd0;
		font-size: 20px;
	}
}
.line {
	display: inline-block;
	width: 15px;
	height: 15px;
	border-radius: 15px;
	background-color: #4b9cdb;
}
.loading_box .line:nth-last-child(1) {
	animation: loadingAni 0.6s 0.1s linear infinite;
}
.loading_box .line:nth-last-child(2) {
	animation: loadingAni 0.6s 0.2s linear infinite;
}
.loading_box .line:nth-last-child(3) {
	animation: loadingAni 0.6s 0.3s linear infinite;
}
@keyframes loadingAni {
	0% {
		transform: translate(0, 0);
	}
	50% {
		transform: translate(0, 15px);
	}
	100% {
		transform: translate(0, 0);
	}
}
</style>
