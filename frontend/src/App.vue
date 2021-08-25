<template>
  <div class="container">
    <div class="row">
      <!-- Title -->
      <h1 class="mt-4">Coin Market</h1>
      <input 
        type="text" 
        class="form-control bg-dark text-light rounded-0 border-0 my-4" 
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      >
      <!-- Table -->
      <table class="table table-dark">
        <!-- Table Head -->
        <thead>
          <tr>
            <th class="text-center" v-for="title in titles" :key="title">
              {{title}}
            </th>
          </tr>
        </thead>
        <!-- Table Body -->
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">
              {{index + 1}}
            </td>
            <td>
              <!-- Logo -->
              <img :src="coin.image" style="width:2rem" class="me-2">
              <!-- Name -->
              <span>
                {{coin.name}}
              </span>
              <!-- Symbol -->
              <span class="ms-2 text-uppercase text-muted">
                {{coin.symbol}}
              </span>
            </td>
            <!-- Price -->
            <td class="text-end">
              $ {{Math.floor(coin.current_price)}} CLP
            </td>
            <!-- Price Variation 24h -->
            <td class="text-end" :class="[coin.price_change_24h > 0 ? 'text-success':'text-danger']">
              $ {{Math.floor(coin.price_change_24h)}} CLP
            </td>
            <!-- Volume 24h -->
            <td class="text-end">
              $ {{coin.total_volume}} CLP
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: [
        '#',
        'Coin',
        'Price',
        'Price Change 24h',
        'Volume 24hr'
      ],
      textSearch: '',
    }
  }, 
  async mounted() {
    const response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=clp&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await response.json()
    console.log(data)
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter((coin) =>
        coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) || coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      )
    },
  }
}
</script>

<style>
</style>
