<script setup lang="ts">
import { computed, ref } from 'vue'

function fmtNum(num: number): string {
  return new Intl.NumberFormat('en-US').format(num)
}

function fmtUSD(num: number, sigfig?: number): string {
  const opts: { [k: string]: any } = { style: 'currency', currency: 'USD' }
  if (sigfig) {
    opts.maximumSignificantDigits = sigfig
  }
  return new Intl.NumberFormat('en-US', opts).format(num)
}

const min = 1_000_000 // 1m
const max = 100_000_000_000 // 100b
const steps = 50

const sMin = Math.log10(min) * 1000
const sMax = Math.log10(max) * 1000
const sStep = (sMax - sMin) / steps

const rCompanyValuation = ref(sMin.toString())
const companyValuation = computed(() => {
  return Math.pow(10, parseInt(rCompanyValuation.value) / 1000)
})

const yourTotalGrant = ref(10000)
const yourGrantPercent = ref(25)
const totalShares = ref(1_000_000_000)
const strikePrice = ref(1.99)

const yourShareCount = computed(() => Math.floor(yourTotalGrant.value * yourGrantPercent.value * 0.01))
const yourCostToExercise = computed(() => yourShareCount.value * strikePrice.value)
const yourHoldingsAtIPO = computed(() => yourShareCount.value / totalShares.value * companyValuation.value)
const yourTotalProfit = computed(() => yourHoldingsAtIPO.value - yourCostToExercise.value)
</script>

<template>
  <div class="card">
    <p>Company valuation at IPO: {{ fmtUSD(companyValuation, 4) }}</p>
    <input type="range" :min="sMin" :max="sMax" :step="sStep" v-model="rCompanyValuation">

    <p>
      Total number of shares (current dilution): {{ fmtNum(totalShares) }}
    </p>
    <p><input type="number" v-model="totalShares"></p>

    <p>
      Number of stock options granted to you: {{ fmtNum(yourTotalGrant) }}
    </p>
    <p><input type="number" v-model="yourTotalGrant"></p>

    <p>% of options that you have vested: {{ yourGrantPercent }}%</p>
    <p><input type="range" min="0" max="100" step="1" v-model="yourGrantPercent"></p>

    <p>Value of your options at time of IPO: {{ fmtUSD(yourHoldingsAtIPO) }}</p>

    <p>Your vested share count: {{ fmtNum(yourShareCount) }}</p>

    <p>Strike price for one option: ${{ strikePrice }}</p>
    <p><input type="number" v-model="strikePrice"></p>
    <p>Your cost to exercise: {{ fmtUSD(yourCostToExercise) }}</p>

    <p>Your total profit: {{ fmtUSD(yourTotalProfit) }}</p>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
