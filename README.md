# VueJS Weather App

### This app is built with:
* [Vue CLI](https://cli.vuejs.org/)
* [MetaWeather API](https://www.metaweather.com/api/)
* [Vuetify](https://vuetifyjs.com/en/)
* [Vue-chartjs](https://vue-chartjs.org/)
* [Axios](https://github.com/axios/axios)
* [CORS Anywhere](https://github.com/Rob--W/cors-anywhere/)
* [Moment.js](https://momentjs.com/)


### UI Display
<p align="center">
  <img src="https://i.imgur.com/6wACgyn.gif" />
</p>

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

Navigate to localhost:8080 to view the application. Ensure CORS is setup to view API responses in browser (see CORS section).

### Compiles and minifies for production
```
yarn build
```

### Setup cross origin requests (CORS)

Since this is a front end only application making cross origin requests, you must first go to https://cors-anywhere.herokuapp.com and allow for cross-origin requests(CORS) to the app. Simply click on button (if one is available on the page). If not, then making API requests out to the MetaWeather API should work.


