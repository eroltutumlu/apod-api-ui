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

      <div>
        <b-button id="show-btn" @click="$bvModal.show('subscribeModal')">Subscribe</b-button>

        <b-modal id="subscribeModal" hide-footer>
          <template #modal-title>
            Get daily email from NASA
          </template>
        <div>
          <b-form @submit="subscribe">

            <b-form-group
              id="inputGroup1"
              label="Email address:"
              label-for="subEmailAddress"
              description="We'll never share your email with anyone else."
            >
              <b-form-input
                id="subEmailAddress"
                v-model="subscribeForm.email"
                type="email"
                required
                placeholder="Enter email"
              ></b-form-input>
            </b-form-group>

            <b-form-group id="inputGroup2" label="Your name:" label-for="subName">
              <b-form-input
                id="subName"
                v-model="subscribeForm.name"
                required
                placeholder="Enter fullname"
              ></b-form-input>
            </b-form-group>

            <b-form-group id="inputGroup3" label="Your surname:" label-for="subSurname">
              <b-form-input
                id="subSurname"
                v-model="subscribeForm.surname"
                required
                placeholder="Enter surname"
              ></b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Subscribe</b-button>
          </b-form>
        </div>
        </b-modal>
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
      apod: Object,
      subscribeForm: {
        email: '',
        name: '',
        surname: ''
      },
      subscriberModalShow: true
    }
  },
  watch: {
    value: function (selectedDate) {
      this.callApi(selectedDate)
    }
  },
  methods: {
    callApi(selectedDate) {
      const url = 'http://localhost:8096/apod?date=' + selectedDate
      fetch(url, {method: 'get'})
      .then((data) => data.json())
      .then((jsonData) => {
        this.apod = jsonData
      })
    },
    formatDate(date){
      return moment(date).format('YYYY-MM-DD')
    },
    subscribe(e) {
      e.preventDefault();
      
      fetch('http://localhost:8096/api/subscribe', {
        method: 'post',
        headers: {
          'Accept': 'application/json, text/plain, */*',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.subscribeForm)
      }).then(res=>res.json())
        .then(res => console.log(res));

    },
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
