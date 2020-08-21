<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
      <div style="width: 500px">
        hi
        <!--
        <vue-monthly-picker
          v-model="selectedMonth"
          @selected="onMonthSelected"
          >
        </vue-monthly-picker>
        -->
        <my-date-picker v-on:callback='callback'></my-date-picker>
    </div>
    </v-main>

    
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
// import VueMonthlyPicker from 'vue-monthly-picker'
import MyDatePicker from './components/datepicker'

import Vue from 'vue'
Vue.component('my-date-picker', MyDatePicker)


export default {
  name: 'App',

  components: {
    HelloWorld,
    MyDatePicker
  },

  data: () => ({
    //
    selectedMonth: null
  }),
  methods: {
    onMonthSelected: function() {
      alert('month selected ' + this.selectedMonth.format('yyyy-MM'))
    },
    callback(evt, resolve) {
      console.log('callback called')
      setTimeout(() => resolve('resolved'), 2000)
    }
  }
};
</script>
