<template>
<div>
  <b-form inline class="mb-4 justify-content-center">
    <!-- amount -->
    <b-form-group label="Amount">
      <b-form-input type="number" min="0" v-model="form.amount" size="lg" class="mb-2 mr-sm-2 mb-sm-2"></b-form-input>
    </b-form-group>

    <!-- base currency -->
    <b-form-group label="From">
      <b-form-select v-model="form.base" :options="baseCurrencies" size="lg" class="mb-2 mr-sm-2 mb-sm-2"></b-form-select>
    </b-form-group>

    <!-- switch currency -->
    <b-button @click="switchBaseTarget" variant="primary" size="lg" class="mb-2 mr-sm-2 mb-sm-2" style="margin-top: 2rem;">
      <BIconArrowLeftRight/>
    </b-button>

    <!-- target currency -->
    <b-form-group label="To">
      <b-form-select v-model="form.target" :options="targetCurrencies" size="lg" class="mb-2 mr-sm-2 mb-sm-2"></b-form-select>
    </b-form-group>
  </b-form>

  <!-- exchange rate information -->
  <p class="h6"><strong>{{form.amount}} {{form.base}} =</strong></p>
  <p class="h3"><strong>{{result}} {{form.target}}</strong></p>
</div>

</template>

<script>
import  { BIconArrowLeftRight }  from 'bootstrap-vue'
import currencyRates from '../json/currency-rates.json'

export default {
  components: {
    BIconArrowLeftRight,
  },
  data() {
    return {
      baseCurrencies: [],
      targetCurrencies: [],
      form: {
        base: null,
        target: null,
        amount: '1.00',
      }

    }
  },
  created() {
    // add inverse currency rates
    Object.values(currencyRates).forEach(currency => {
      if (currency.targets) {
        let targetCurrencies = Object.entries(currency.targets)
        targetCurrencies.forEach(targetCurrency => {
          let [currencyCode, rate] = targetCurrency

          if (!currencyRates[currencyCode].targets) currencyRates[currencyCode].targets = {}
          currencyRates[currencyCode].targets[currency.code] = parseFloat(1/rate).toFixed(4)
        })
      }
    }) 

    this.baseCurrencies = Object.values(currencyRates).map(currency => ({value: currency.code, text: `${currency.code} - ${currency.name}`}))
    this.form.base = this.baseCurrencies[0].value
  },

  methods: {
    setTargetCurrencies() {
      let targetCurrencies = []
      const availableCurrencies = Object.keys(currencyRates[this.form.base].targets)

      Object.values(currencyRates).forEach(currency => {
        if (availableCurrencies.includes(currency.code)) targetCurrencies.push({value: currency.code, text: `${currency.code} - ${currency.name}`})
      })

      this.targetCurrencies = targetCurrencies
      if (targetCurrencies.length && !availableCurrencies.includes(this.form.target)) {
        this.form.target = targetCurrencies[0].value
      }
    },
    switchBaseTarget() {
      let temp = this.form.base
      this.form.base = this.form.target
      this.form.target = temp
    }
  },
  watch: {
    'form.base'() {
      this.setTargetCurrencies()
    }
  },
  computed: {
    result() {
      let rate = currencyRates[this.form.base].targets[this.form.target]
      return parseFloat(rate * this.form.amount).toFixed(4)
    }
  }
  

}
</script>

<style>

</style>