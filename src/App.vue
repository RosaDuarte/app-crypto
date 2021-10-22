<template>
  <div class="container">
    <div class="row">
      <div class="block pt-4">
        <h1 class="title is-2 has-text-centered">Mercado de monedas</h1>
      </div>
      <div class="block">
        <input type="text" class="input" placeholder="Buscador de monedas" @keyup="searchCoin()" v-model="textSearch">
      </div>
      <div class="table-container">
        <table class="table">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title" class="has-text-centered">
              {{title}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="has-text-centered">
              {{index + 1}}
            </td>
            <td class="has-text-centered">
              <img :src="coin.image" alt="" class="mr-2">
              <span>
                {{coin.name}}
              </span>
              <span class="ml-2 is-uppercase">
                {{coin.symbol}}
              </span>
            </td>
            <td class="has-text-centered">
              $ {{coin.current_price}}
            </td>
            <td v-bind:class="[coin.price_change_percentage_24h > 0 ? 'has-text-success' : 'has-text-danger']" class="has-text-centered">
              {{coin.price_change_percentage_24h}} %
            </td>
            <td class="has-text-centered">
              $ {{coin.total_volume.toLocaleString()}}
            </td>
          </tr>
        </tbody>
      </table>
      </div>
    </div>
    <footer class="block">
      <div class="content has-text-centered">
        <p class="subtitle is-5">Crypto 2021</p>
      </div>
    </footer>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      coins: [],
      filteredCoins: [],
      titles: [
        '#',
        'Moneda',
        'Precio',
        'Cambio de precio',
        'Volumen 24h'
      ],
      textSearch: ''
    }
  },
  async mounted(){
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json();
    this.coins = data;
    this.filteredCoins = data
  },
  methods:{
    searchCoin(){
    this.filteredCoins= this.coins.filter((coin) => 
    coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) || coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
    )
  }
  }
}
</script>

<style>
  html{
    background-image: linear-gradient(0, #2b8f80 0, #247f76 16.67%, #206c69 33.33%, #1f5959 50%, #1e4648 66.67%, #1c3638 83.33%, #1a282a 100%);
  }
  .block{
    margin: auto;
    text-align: center;
  }
  .block h1{
    color: #0ffff3;
    width: 50%;
    margin: auto;
    border: 0; 
    box-shadow: inset 0 -12px 12px -12px #b0e6e5;
  }
  .block input{
    width: 50%;
    background-color: transparent;
    box-shadow:  0px 2px 2px 1px #aaab99;
    color: #00edff;
  }
  .block input::placeholder{
    color: #0ffff3;
  }
  .block input:focus{
    box-shadow:  0px 2px 2px 1px #aaab99;
    border: 1px solid #0ffff3;
  }
  .table-container table{
    margin: auto;
    border-radius: 5px;
    background-color: transparent;
    color: #00edff;
    
  }
  .table-container table th{
    color: #0ffff3;
  }
  img{
    width: 15px;
  }
  footer{
    padding-top: 10px;
    padding-bottom: 10px;
    background-color: transparent;
    width: 20%;
    margin: auto;
    box-shadow: inset 0 -12px 12px -12px #b0e6e5;
  }
</style>
