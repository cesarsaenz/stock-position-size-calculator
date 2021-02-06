<template>
  <div>
    <fieldset class="mt-2">
      <legend class="block text-xs font-medium text-gray-500">Details (ticker, entry, stop, risk, step)</legend>
      <div class="mt-1 bg-white rounded-md shadow-sm -space-y-px">
        <div class="flex -space-x-px">
          <div class="w-1/2 flex-1 min-w-0">
            <label for="ticker" class="sr-only">Ticker</label>
            <input v-model="ticker" v-uppercase name="ticker"  class="pl-2 focus:ring-indigo-500 focus:border-indigo-500 relative block w-full rounded-none rounded-bl-md bg-transparent focus:z-10 text-2xl sm:text-lg border-gray-300" placeholder="ticker">
          </div>
          <div class="w-1/2 flex-1 min-w-0">
            <label for="entry" class="sr-only">Entry</label>
            <input v-model="entry" @keypress="isNumber($event)" type="number" min="1" name="entry" id="entry" class="pl-2 focus:ring-indigo-500 focus:border-indigo-500 relative block w-full rounded-none rounded-bl-md bg-transparent focus:z-10 text-2xl sm:text-lg border-gray-300" placeholder="entry">
          </div>
          <div class="flex-1 min-w-0">
            <label for="stop" class="sr-only">Stop</label>
            <input v-model="stop" @keypress="isNumber($event)" type="number" min="1" name="stop" id="stop" class="pl-2 focus:ring-indigo-500 focus:border-indigo-500 relative block w-full rounded-none rounded-br-md bg-transparent focus:z-10 text-2xl sm:text-lg border-gray-300" placeholder="stop">
          </div>
          <div class="flex-1 min-w-0">
            <label for="risk" class="sr-only">Risk</label>
            <input v-model="risk" @keypress="isNumber($event)" type="number" min="1" name="risk" id="risk" class="pl-2 focus:ring-indigo-500 focus:border-indigo-500 relative block w-full rounded-none rounded-br-md bg-transparent focus:z-10 text-2xl sm:text-lg border-gray-300" placeholder="risk">
          </div>
          <div class="flex-1 min-w-0">
            <label for="step" class="sr-only">Step</label>
            <input v-model="step" @keypress="isNumber($event)" type="number" min="1" name="step" id="step" class="pl-2 focus:ring-indigo-500 focus:border-indigo-500 relative block w-full rounded-none rounded-br-md bg-transparent focus:z-10 text-2xl sm:text-lg border-gray-300" placeholder="step">
          </div>
        </div>
      </div>
    </fieldset>
    <div class="overflow-x-auto">
      <table class="mt-4" v-if="hasResults" >
      <tr>
        <td class="px-1 py-1 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">risk</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getRiskForRow(0) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getRiskForRow(1) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getRiskForRow(2) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getRiskForRow(3) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getRiskForRow(4) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getRiskForRow(5) }}</td>
      </tr>
      <tr>
        <td class="px-1 py-1 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">buy</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(0) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(1) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(2) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(3) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(4) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(5) }}</td>
      </tr>
      <tr>
        <td class="px-1 py-1 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">cost</td>
        <td class="px-6 py-1 text-right text-sm font-semibold text-red-500">{{ getCostForRow(0) }}</td>
        <td class="px-6 py-1 text-right text-sm font-semibold text-red-500">{{ getCostForRow(1) }} </td>
        <td class="px-6 py-1 text-right text-sm font-semibold text-red-500">{{ getCostForRow(2) }}</td>
        <td class="px-6 py-1 text-right text-sm font-semibold text-red-500">{{ getCostForRow(3) }}</td>
        <td class="px-6 py-1 text-right text-sm font-semibold text-red-500">{{ getCostForRow(4) }}</td>
        <td class="px-6 py-1 text-right text-sm font-semibold text-red-500">{{ getCostForRow(5) }}</td>
      </tr>
      <tr>
        <td class="px-1 py-1 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
          <div class="">tier1</div>
          <span class="flex-shrink-0 inline-block px-1 py-0.5 text-green-800 text-xs font-medium bg-green-100 rounded-full">25%</span>
        </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierOne, 0) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierOne, 1) }}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierOne, 2)}}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierOne, 3)}}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierOne, 4)}}</td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierOne, 5)}}</td>
      </tr>
      <tr>
        <td class="px-1 py-1 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
          <div class="">tier2</div>
          <span class="flex-shrink-0 inline-block px-1 py-0.5 text-green-800 text-xs font-medium bg-green-100 rounded-full">50%</span>
        </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierTwo, 0) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierTwo, 1) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierTwo, 2) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierTwo, 3) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierTwo, 4) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getTierForFow(this.tierTwo, 5) }} </td>
      </tr>
      <tr>
        <td class="px-1 py-1 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
          <div class="">tier3</div>
          <span class="flex-shrink-0 inline-block px-1 py-0.5 text-green-800 text-xs font-medium bg-green-100 rounded-full">25%</span>
        </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(0) - getTierForFow(this.tierOne, 0) - getTierForFow(this.tierTwo, 0) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(1) - getTierForFow(this.tierOne, 1) - getTierForFow(this.tierTwo, 1) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(2) - getTierForFow(this.tierOne, 2) - getTierForFow(this.tierTwo, 2) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(3) - getTierForFow(this.tierOne, 3) - getTierForFow(this.tierTwo, 3) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(4) - getTierForFow(this.tierOne, 4) - getTierForFow(this.tierTwo, 4) }} </td>
        <td class="px-6 py-1 text-right text-2xl font-semibold text-gray-900">{{ getBuyForRow(5) - getTierForFow(this.tierOne, 5) - getTierForFow(this.tierTwo, 5) }} </td>
      </tr>
    </table>
    </div>
  </div>
</template>

<script >

export default {
  name: 'StockPositionSizeCalculator',
  props: ['tickerPosition'],
  data: function () {
    return {
      ticker: '',
      entry: '',
      stop: '',
      risk: '',
      step: '',
      sizeToBuy: 0,
      tierOne: 0.25,
      tierTwo: 0.50,
      tierThree: 0.25
    }
  },
  mounted () {
    if (localStorage.getItem('ticker-' + this.tickerPosition)) {
      this.ticker = localStorage.getItem('ticker-' + this.tickerPosition)
    }

    if (localStorage.getItem('risk-' + this.tickerPosition)) {
      this.risk = localStorage.getItem('risk-' + this.tickerPosition)
    }

    if (localStorage.getItem('entry-' + this.tickerPosition)) {
      this.entry = localStorage.getItem('entry-' + this.tickerPosition)
    }

    if (localStorage.getItem('stop-' + this.tickerPosition)) {
      this.stop = localStorage.getItem('stop-' + this.tickerPosition)
    }

    if (localStorage.getItem('step-' + this.tickerPosition)) {
      this.step = localStorage.getItem('step-' + this.tickerPosition)
    }
  },
  computed: {
    hasResults: function () {
      return this.entry && this.stop && this.risk && this.step
    }
  },
  watch: {
    ticker (newTicker) {
      localStorage.setItem('ticker-' + this.tickerPosition, newTicker.toUpperCase())      
      this.$emit('tickerNameSet', this.tickerPosition, newTicker.toUpperCase())
    },
    risk (newRisk) {
      localStorage.setItem('risk-' + this.tickerPosition, newRisk)
    },
    entry (newEntry) {
      localStorage.setItem('entry-' + this.tickerPosition, newEntry)
    },
    stop (newStop) {
      localStorage.setItem('stop-' + this.tickerPosition, newStop)
    },
    step (newStep) {
      localStorage.setItem('step-' + this.tickerPosition, newStep)
    }
  },
  methods: {
    isNumber (evt) {
      var charCode = (evt.which) ? evt.which : evt.keyCode
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault()
      } else {
        return true
      }
    },
    getRiskForRow (row) {
      return parseInt(this.risk) + parseInt(this.step) * row
    },
    getBuyForRow (row) {
      return Math.round((parseInt(this.risk) + parseInt(this.step) * row) / (parseFloat(this.entry) - parseFloat(this.stop)))
    },
    getCostForRow (row) {
      return Math.trunc(100 * (parseFloat(this.entry) * Math.round((parseInt(this.risk) + parseInt(this.step) * row) / (parseFloat(this.entry) - parseFloat(this.stop))))) / 100
    },
    getTierForFow (tierValue, row) {
      return Math.round(tierValue * Math.round((parseInt(this.risk) + parseInt(this.step) * row) / (parseFloat(this.entry) - parseFloat(this.stop))))
    }
  },
  directives: {
    uppercase: function (el) {
      el.value = el.value.toUpperCase()
    }
  }
}
</script>
