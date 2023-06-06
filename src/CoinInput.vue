<template>
  <div>
    <input v-model="searchText" @input="fetchCoins" placeholder="Введите название монеты">
    <span v-for="coin in coins" :key="coin.id">{{ coin.name }}</span>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchText: '',
      coins: [],
    };
  },
  methods: {
    fetchCoins() {
      axios.get('https://min-api.cryptocompare.com/data/all/coinlist?summary=true')
        .then(response => {
          const coinList = response.data.Data;
          this.coins = Object.values(coinList).filter(coin => coin.name.includes(this.searchText));
        })
        .catch(error => {
          console.error(error);
        });
    },
  },
};
</script>
