<template>
  <div class="container">
    <div class="title-container">
      <h2 class="title">{{ coin.name }} Detail Page</h2>
      <button @click="goBack" class="go-back-button">Go Back</button>
    </div>
    <button @click="toggleMode" class="mode-toggle">{{ buttonText }}</button>
    <div class="table-container">
      <table class="currency-table">
        <thead>
          <tr>
            <th>Symbol</th>
            <th>Rank</th>
            <th>Price (USD)</th>
            <th>Market Cap (USD)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{{ coin.symbol }}</td>
            <td>{{ coin.rank }}</td>
            <td>{{ coin.price_usd }}</td>
            <td>{{ coin.market_cap_usd }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
const route = useRoute()
const { data } = await useFetch('/api/ticker/?id=' + route.params.id)
const coin = data.value[0]

let isDarkMode = false;
const buttonText = ref('Dark Mode'); // Initialize button text

const toggleMode = () => {
  isDarkMode = !isDarkMode;
  if (isDarkMode) {
    document.documentElement.classList.add('dark-mode');
    document.body.style.backgroundColor = '#333';
    document.body.style.color = '#ddd';
    buttonText.value = 'Light Mode'; // Update button text
  } else {
    document.documentElement.classList.remove('dark-mode');
    document.body.style.backgroundColor = '#fff';
    document.body.style.color = '#333';
    buttonText.value = 'Dark Mode'; // Update button text
  }
}

const router = useRouter();

const goBack = () => {
  router.go(-1); // Go back one step in the history stack
}
</script>

<style scoped>
.container {
  width: 100%;
  max-width: 800px; /* Set maximum width */
  margin: 0 auto; /* Center horizontally */
  padding: 20px;
}

.title-container {
  width: 100%;
  text-align: center;
  margin-bottom: 20px;
}

.title {
  font-size: 32px;
  margin: 0;
}

.go-back-button {
  padding: 10px 20px;
  margin-bottom: 10px; /* Added margin */
  border: none;
  background-color: #0066cc;
  color: #fff;
  cursor: pointer;
  border-radius: 5px;
}

.go-back-button:hover {
  background-color: #0052a3;
}

.mode-toggle {
  padding: 10px 20px;
  border: none;
  background-color: #333;
  color: #fff;
  cursor: pointer;
  border-radius: 5px;
  position: absolute;
  top: 20px;
  right: 20px;
}

.mode-toggle:hover {
  background-color: #444;
}

.table-container {
  width: 100%;
  overflow-x: auto;
}

.currency-table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #666;
  background-color: #fff;
}

.currency-table th, .currency-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #666;
}

.currency-table th {
  background-color: #f0f0f0;
  font-weight: bold;
}

.currency-table tr:nth-child(even) {
  background-color: #f5f5f5;
}

.currency-table tr:hover {
  background-color: #e5e5e5;
}

.dark-mode .currency-table {
  background-color: #444;
  color: #ddd;
}

.dark-mode .currency-table th {
  background-color: #555;
}

.dark-mode .currency-table tr:nth-child(even) {
  background-color: #555;
}

.dark-mode .currency-table tr:hover {
  background-color: #666;
}
</style>
