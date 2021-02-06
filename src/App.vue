<template>
<div class="max-w-2xl mx-auto text-center p-6">
  <h1 class="text-gray-800 text-2xl font-light">Stock Position Size Calculator</h1>
  <stock-position-size-calculator class="rounded-md shadow-xl" tickerPosition="1" />
  <stock-position-size-calculator class="mt-4 rounded-md shadow-xl" tickerPosition="2" />
</div>    
</template>

<script>
import StockPositionSizeCalculator from '@/components/StockPositionSizeCalculator.vue'
export default {
  name:'App',
  components: { StockPositionSizeCalculator }
}
</script>

<style>
html {
      @apply  border-t-4 border-indigo-600 bg-gray-50
    } 
</style>
