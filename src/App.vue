<template>
  <div>
    <main>
      <Panel class="calculator-panel" headline="Income Tax Calculator">
        <template>
          <transition-group
            name="alert-in"
            mode="out-in"
            enter-active-class="animated fadeIn"
            leave-active-class="animated fadeOut"
          >
            <InputForm key="inputform" @submitted="submitted"/>
            <Result
              key="result"
              v-if="resultsCalculated"
              @clearCalculations='clearCalculations'
              :results='calculations'
            />
          </transition-group>  
        </template>
      </Panel>
    </main>
  </div>
</template>

<script>
import Panel from './components/Panel.vue'
import InputForm from './components/InputForm.vue'
import Result from './components/Result.vue'
import { calcTaxes } from './calc.js'

export default {
  name: 'app',
  components: {
    Panel,
    InputForm,
    Result
  },
  data () {
    return {
      calculations: {}
    }
  },

  computed: {
    resultsCalculated: function () {
      return Object.keys(this.calculations).length !== 0
    }
  },

  methods: {
    submitted: function (input) {
      const calcValues = calcTaxes(input)

      this.calculations = {
        grossIncome: { label: 'Gross Salary', value: calcValues.incomeValue },
        tax: { label: 'Income Tax', value: -calcValues.incomeTax },
        churchTax: { label: 'Church Tax', value: -calcValues.churchTax },
        soli: { label: 'Solidarity Charge', value: -calcValues.soli },
        netIncome: { label: 'Net Salary', value: calcValues.netIncome }
      }
    },
    clearCalculations: function () {
      this.calculations = {}
    }
  }
}
</script>

<style lang="scss" src="./assets/styles/App.scss"/>