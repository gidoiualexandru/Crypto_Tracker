<template>
  <div :class="{ 'dark-mode': isDarkMode }">
    <button @click="toggleMode" class="mode-toggle">{{ buttonText }}</button>
    <main>
      <h1 class="title">Top 15 Cryptocurrencies</h1>

      <div class="table-container">
        <table class="currency-table">
          <thead>
            <tr>
              <th>Name</th>
              <th>Symbol</th>
              <th>Price (USD)</th>
              <th>Details</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="currency in data.data" :key="currency.id" class="currency-row">
              <td>{{ currency.name }}</td>
              <td>{{ currency.symbol }}</td>
              <td>{{ currency.price_usd }}</td>
              <td>
                <NuxtLink :to="'/currency/' + currency.id" class="details-link">View Details</NuxtLink>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </main>
  </div>
</template>

<script setup>
const { data } = await useFetch('/api/tickers/?limit=15 ')
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
</script>
<style scoped>
/* Default light mode styles */
body {
  margin: 0;
  font-family: 'Roboto', sans-serif;
}

.title {
  font-size: 36px;
  margin-top: 20px;
  margin-bottom: 20px;
  text-align: center;
}

.table-container {
  width: 100%;
  overflow-x: auto;
}

.currency-table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #666;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
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

.currency-row:nth-child(even) {
  background-color: #f5f5f5;
}

.currency-row:hover {
  background-color: #e5e5e5;
}

.details-link {
  color: #0066cc;
  text-decoration: none;
  font-weight: bold;
}

.details-link:hover {
  text-decoration: underline;
}

/* Dark mode styles */
.dark-mode .currency-table {
  background-color: #444;
}

.dark-mode .currency-table th {
  background-color: #555;
}

.dark-mode .currency-row:nth-child(even) {
  background-color: #555;
}

.dark-mode .currency-row:hover {
  background-color: #666;
}

.dark-mode .details-link {
  color: #00ccff;
}

.mode-toggle {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 10px 20px;
  border: none;
  background-color: #333;
  color: #fff;
  cursor: pointer;
  border-radius: 5px;
}

.mode-toggle:hover {
  background-color: #444;
}
</style>
