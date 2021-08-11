<template>
  <div id="app">
      <b-jumbotron header="Currency Converter">
        <hr class="my-4">
        <b-container>
          <b-row>
            <b-col>
              <CurrencyConverter/>
            </b-col>
          </b-row>
        </b-container>
      </b-jumbotron>
  </div>
</template>

<script>
import CurrencyConverter from './components/CurrencyConverter.vue'

export default {
  name: 'App',
  components: {
    CurrencyConverter
  }
}
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
