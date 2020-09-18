<template>
  <div id="app">
    <b-container style="margin-bottom: 30px">
      <div>
        <label for="example-datepicker">Choose a date</label>
        <b-form-datepicker
          id="example-datepicker"
          v-model="value"
          class="mb-2"
        ></b-form-datepicker>

      </div>
      <div>
        <b-card v-if="apod">
            <b-img :src="apod.url" fluid-grow rounded  alt="apod.title"></b-img>
          <b-card-title class="mt-2">{{apod.title}}</b-card-title>
          <b-card-text>
            {{apod.explanation}}
          </b-card-text>
        </b-card>
      </div>
    </b-container>
  </div>
</template>

<script>

import moment from 'moment'

export default {
  name: "App",
  components: {},
  data() {
    return {
      value: new Date(),
      selected: '',
      apod: Object
    }
  },
  watch: {
    value: function (selectedDate) {
      this.callApi(selectedDate)
    }
  },
  methods: {
    callApi(selectedDate) {
      const url = 'https://apod-api-app.herokuapp.com/apod?date=' + selectedDate
      fetch(url, {method: 'get'})
      .then((data) => data.json())
      .then((jsonData) => {
        this.apod = jsonData
      })

    },
    formatDate(date){
      return moment(date).format('YYYY-MM-DD')
    }
  },
  mounted: function() {
    let selectedDate = this.value
    let formattedDate = this.formatDate(selectedDate)
    this.callApi(formattedDate)
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
