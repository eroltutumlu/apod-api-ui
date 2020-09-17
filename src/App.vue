<template>
  <div id="app">
    <b-container>
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
            <b-img :src="apod.url" fluid alt="apod.title"></b-img>
          <b-card-title>{{apod.title}}</b-card-title>
          <b-card-text>
            {{apod.explanation}}
          </b-card-text>
        </b-card>
      </div>
    </b-container>
  </div>
</template>

<script>
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
      const url = 'http://localhost:8095/apod?date=' + selectedDate
      fetch(url, {method: 'get'})
      .then((data) => data.json())
      .then((jsonData) => {
        this.apod = jsonData
      })

    }
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
