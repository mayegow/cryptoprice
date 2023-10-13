<template>
  <div class="container">
    <div class="row">
      <h1>Crypto Market</h1>
      <div class="form-floating mb-3">
        <input type="email" class="form-control border-0 rounded-0 bg-dark text-secondary" id="floatingInput" placeholder="search coin" @keyup="searchCoin" v-model="coinSearch">
        <label for="floatingInput">Search coin</label>
      </div>
      <table class="table table-secondary">
        <thead>
          <tr >
            <th v-for="title, idx in titles" :key="idx">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="coin, idx in filteredCoins" :key="coin.id">
            <td>
              {{ idx + 1 }}
            </td>
            <td>
              <img :src="coin.image" style="width:2rem;" class="me-2" />
              <span>
                {{coin.name}}
              </span>
              <span class="ms-2 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>
              ${{ coin.current_price }}
            </td>
            <td :class="coin.price_change_percentage_24h < 0 ? 'text-danger' : 'text-success' ">
              {{ coin.price_change_percentage_24h }}%
            </td>
            <td>
              ${{ coin.total_volume.toLocaleString() }}
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
  data(){
    return{
      coins: [],
      filteredCoins:[],
      titles: [
        "#",
        "Coin",
        "Price",
        "Price Change",
        "24h Volume"

      ],
      coinSearch: "",
    }
  },
  async mounted(){
    const res = await fetch("https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&locale=en")
    const data = await res.json()
    this.coins = data
    this.filteredCoins = data
  },
  methods:{
    searchCoin() {
      this.filteredCoins=this.coins.filter((coin) => 
      coin.name.toLowerCase().includes(this.coinSearch.toLowerCase()) ||
      coin.symbol.toLowerCase().includes(this.coinSearch.toLowerCase())) 
    }

  }
  
}
</script>

<style>

</style>
