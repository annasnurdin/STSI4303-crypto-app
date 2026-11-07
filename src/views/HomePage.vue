<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Daftar Cryptocurrency</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <div class="button-container">
        <ion-button class="refresh-btn" @click="loadData">
          Refresh
        </ion-button>
      </div>
      <table class="crypto-table">
        <thead>
          <tr>
            <th class="text-center">Rank</th>
            <th>Coin</th>
            <th>Price (USD)</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="coin in cryptos" :key="coin.id">
            <td class="text-center rank-col">{{ coin.rank }}</td>
            <td>
              <div class="symbol">{{ coin.symbol }}</div>
              <div class="name">{{ coin.name }}</div>
            </td>
            <td class="price-col">{{ coin.price_usd }}</td>
          </tr>
        </tbody>
      </table>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton } from '@ionic/vue';
import axios from 'axios';

interface Crypto {
  id: string;
  rank: number;
  name: string;
  symbol: string;
  price_usd: string;
}

const cryptos = ref<Crypto[]>([]);

const loadData = async () => {
  try {
    const response = await axios.get('https://api.coinlore.net/api/tickers/');
    cryptos.value = response.data.data;
  } catch (error) {
    console.error('Gagal mengambil data', error);
  }
};

onMounted(() => {
  loadData();
});
</script>

<style scoped>
.button-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.refresh-btn {
  --padding-top: 14px;
  --padding-bottom: 14px;
  --padding-start: 32px;
  --padding-end: 32px;
  font-size: 18px;
  font-weight: bold;
}

.crypto-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 17px;
}

.crypto-table th,
.crypto-table td {
  border: 1px solid #ddd;
  padding: 14px 12px;
}

.crypto-table th {
  background-color: #f4f5f8;
  color: #222;
  font-size: 18px;
  font-weight: 700;
  text-align: left;
}

.crypto-table th.text-center,
.crypto-table td.text-center {
  text-align: center;
}

.crypto-table th.text-right,
.crypto-table td.text-right {
  text-align: right;
}

.rank-col {
  font-size: 18px;
  font-weight: bold;
  width: 15%;
}

.symbol {
  font-size: 20px;
  font-weight: bold;
  color: #111;
  line-height: 1.2;
}

.name {
  font-size: 15px;
  color: #555;
  margin-top: 4px;
}

.price-col {
  font-size: 18px;
  font-weight: 700;
}
</style>
