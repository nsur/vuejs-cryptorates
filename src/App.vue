<template>
  <div id="app">
    <header>
      <h1>Crypto Currensis Rates</h1>
    </header>
    <main>
      <table class="cryptoCurrencyRates" v-if="rates">
        <thead>
        <tr>
          <th>Image</th>
          <th>Full name</th>
          <th>Symbol</th>
          <th>Price</th>
          <th>Tokens count</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="item in rates.Data">
          <td><img :src="base + item.CoinInfo.ImageUrl" :alt="item.CoinInfo.FullName" /></td>
          <td>{{ item.CoinInfo.FullName }}</td>
          <td>{{ item.CoinInfo.Name }}</td>
          <td>{{ item.DISPLAY.USD.PRICE }}</td>
          <td>{{ item.DISPLAY.USD.SUPPLY }}</td>
        </tr>
        </tbody>
      </table>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      rates: null,
      endpoint: 'https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD',
      base: 'https://www.cryptocompare.com/',
    }
  },
  created() {
    this.getRates();
    setInterval(function () {
      this.getRates();
    }.bind(this), 300000);
  },
  methods: {
    getRates() {
      this.axios.get(this.endpoint)
        .then(response => {
          let data = response.data;
          data.Data.sort((a, b) => { return b.RAW.USD.PRICE - a.RAW.USD.PRICE; });
          this.rates = data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
}
</script>

<style>
  html {
    height: 100%;
    font-size: 18px;
  }
  body {
    height: 100%;
    background-color: #f1f2f7;
    background-position: center;
    background-repeat: no-repeat;
  }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  h1, h2 {
    font-weight: normal;
  }
  h1 {
    font-size: 2rem;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  .cryptoCurrencyRates {
    width: 70%;
    margin: 0 auto;
    border-spacing: 0;
    border-collapse: separate;
    font-size: 14px;
    background-color: #ffffff;
    margin-bottom: 60px;
  }
  .cryptoCurrencyRates {
    border: 1px solid #2c3e50;
  }
  .cryptoCurrencyRates th {
    background-color: #444444;
    color: #ffffff;
    text-transform: uppercase;
  }
  .cryptoCurrencyRates th, .cryptoCurrencyRates td {
    padding: 10px;
    border-right: 1px solid #2c3e50;
    border-bottom: 1px solid #2c3e50;
  }
  .cryptoCurrencyRates tr th:last-child, .cryptoCurrencyRates tr td:last-child {
    border-right: none;
  }
  .cryptoCurrencyRates tbody tr:last-child td {
    border-bottom: none;
  }
  .cryptoCurrencyRates img {
    width: 50px;
    height: auto;
  }
  @media only screen and (max-width: 768px) {
    html {
      font-size: 10px;
    }
    .cryptoCurrencyRates {
      width: 100%;
    }
  }
</style>
