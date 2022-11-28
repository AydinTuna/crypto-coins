<template>
  <div class="coin-container">
    <div class="coin-data">
      <table>
        <tr>
          <th>Coin</th>
          <th>Price</th>
        </tr>
        <tr v-for="coin in coins" :key="coin">
          <td>
            <ul>
              <li>{{ coin.symbol }}</li>
            </ul>
          </td>
          <td>{{ coin.price }}</td>
        </tr>
      </table>
    </div>
    <div class="coin-data-reset">
      <button @click="fetchData">Reset</button>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";

let coins = ref([]);
const url = ref("https://binance43.p.rapidapi.com/ticker/price");

const options = {
  method: "GET",
  headers: {
    "X-RapidAPI-Key": process.env.VUE_APP_BINANCE_API_KEY,
    "X-RapidAPI-Host": "binance43.p.rapidapi.com",
  },
};

const fetchCoins = (response) => {
  coins.value = [];
  response.forEach((element) => {
    coins.value.push(element);
  });
};

onMounted(() => {
  fetchData();
  fetching();
});

function fetching() {
  setInterval(fetchData, 5000);
}

function fetchData() {
  fetch(url.value, options)
    .then((response) => response.json())
    .then((response) => fetchCoins(response))
    .catch((err) => console.error(err));
}
</script>

<style lang="scss">
.coin-container {
  display: flex;
  justify-content: center;
  align-items: center;

  .coin-data {
    table {
      border-collapse: collapse;
      text-align: center;

      tr:nth-child(even) {
        background-color: #dddddd;
      }

      td,
      th {
        border: 1px solid #b8b8b8;
        padding: 8px;
      }

      td {
        &:hover {
          background: #c5c5c5;
        }
      }

      ul {
        padding: 0;
        margin: 0.5rem 0;
        li {
          list-style: none;
        }
      }
    }
  }

  .coin-data-reset {
    align-self: flex-start;

    button {
      margin-left: 1rem;
      width: 78px;
      height: 39px;
      border-radius: 5px;
      border: 1px solid #b8b8b8;

      &:hover {
        cursor: pointer;
        background: #c5c5c5;
      }
    }
  }
}
</style>
