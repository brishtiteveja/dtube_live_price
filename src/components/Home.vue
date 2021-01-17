
<template>
<div class="container" id="app">
      <h1 class="text-center">Live price</h1>
      <div class="columns medium-4" >
        <div class="card">
          <div class="card-section">
            <h2> <a href="https://info.uniswap.org/token/0xd2be3722b17b616c51ed9b8944a227d1ce579c24"> @Uniswap </a> from <a href="https://www.coingecko.com/en/coins/dtube-coin">Coingecko</a></h2>
          </div>
          <div class="card-divider">
            <p>1 <b>DTUBE</b> = <b>{{ cg_info1.data[Contract]["usd"] }} $</b> </p>
            <p>1 <b>DTUBE</b> = <b>{{ cg_info2.data[Contract]["btc"] }} BTC</b> </p>
            <p>1 <b>DTUBE</b> = <b>{{ cg_info3.data[Contract]["eth"] }} ETH</b></p>
          </div>
          <div class="card-section">
            <h2><a href="https://whitebit.com/trade/DTUBE_USDT">@Whitebit</a></h2>
            <br/>
            (Coming soon)
          </div>
<!--
          <div class="card-divider">
            <p>1 <b>DTUBE</b> = <b>{{ wb_info1["last"] }} $ </b>  <br/>(Ask: {{ wb_info1["ask"] }} $, Bid: {{ wb_info1["bid"] }} $)</p>
          </div>
-->
          <div class="card-section">
            <h2><a href="https://exchange.ionomy.com/en/markets/btc-dtube">@Ionomy</a></h2>
          </div>
            <br/>
            (Coming soon)
<!--
          <div class="card-divider">
            <p>1 <b>DTUBE = {{ ion_info1["price"] }} BTC </b> <br/>(Low: {{ ion_info1["low"] }} BTC, High: {{ ion_info1["high"] }} BTC)</p>
          </div>
-->
        </div>
      </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  props: {
  },
  data: function () {
    return {
      cg_info1: null,
      cg_info2: null,
      cg_info3: null,
      wb_info1: null,
      ion_info1: null,
      Contract: '0xd2be3722b17b616c51ed9b8944a227d1ce579c24'
    }
  },
  mounted () {
    axios
      .get('https://api.coingecko.com/api/v3/simple/token_price/ethereum?include_market_cap=true&include_24hr_vol=true&include_last_updated_at=true&contract_addresses=0xd2be3722b17b616c51ed9b8944a227d1ce579c24&vs_currencies=usd')
      .then(response => {
        this.cg_info1 = response
      })
    axios
      .get('https://api.coingecko.com/api/v3/simple/token_price/ethereum?include_market_cap=true&include_24hr_vol=true&include_last_updated_at=true&contract_addresses=0xd2be3722b17b616c51ed9b8944a227d1ce579c24&vs_currencies=btc')
      .then(response => {
        this.cg_info2 = response
      })
    axios
      .get('https://api.coingecko.com/api/v3/simple/token_price/ethereum?include_market_cap=true&include_24hr_vol=true&include_last_updated_at=true&contract_addresses=0xd2be3722b17b616c51ed9b8944a227d1ce579c24&vs_currencies=eth')
      .then(response => {
        this.cg_info3 = response
      })
    var config = {
      headers: {'Access-Control-Allow-Headers': '*', 'Access-Control-Allow-Origin': 'https://www.whitebit.com', 'Access-Control-Allow-Methods': 'OPTIONS,POST,GET'}
    }
    axios
      .get('https://whitebit.com/api/v1/public/ticker?market=DTUBE_USDT', config)
      .then(response => {
        console.log(response)
        this.wb_info1 = response['result']
      })
    axios
      .get('https://ionomy.com/api/v1/public/markets-summaries')
      .then(response => {
        var res = response['data']['data']
        var n = res.length
        for (var i = 0; i < n; i++) {
          var mkt = res[i]['market']
          if (mkt === 'btc-dtube') {
            this.ion_info1 = res[i]
          }
        }
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
</style>
