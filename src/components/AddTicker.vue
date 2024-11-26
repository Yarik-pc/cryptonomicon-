<template>
  <section>
    <div class="flex">
      <div class="max-w-xs">
        <label for="wallet" class="block text-sm font-medium text-gray-700"
          >Тикер</label
        >
        <div class="mt-1 relative rounded-md shadow-md">
          <input
            v-model="ticker"
            @input="setInputValue"
            @keydown.enter="add"
            type="text"
            name="wallet"
            id="wallet"
            class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md py-2 px-2"
            placeholder="Например DOGE"
          />
        </div>
        <div
          v-if="filteredSuggestions.length"
          class="flex bg-white shadow-md p-1 rounded-md flex-wrap"
        >
          <span
            v-for="(suggestion, idx) in filteredSuggestions"
            :key="idx"
            @click="selectSuggestion(suggestion)"
            class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
          >
            {{ suggestion }}
          </span>
        </div>
        <div v-if="error" class="text-sm text-red-600">
          {{ error }}
        </div>
      </div>
    </div>
    <add-button @click="add" type="button" class="my-4" />
  </section>
</template>

<script>
import AddButton from "./AddButton.vue";

export default {
  components: { AddButton },

  props: {
    allCoins: {
      type: Array,
      required: true,
    },

    tickers: {
      type: Array,
      required: true,
    },
  },

  data() {
    return {
      ticker: "",
      filteredSuggestions: [],
      error: null,
    };
  },

  methods: {
    setInputValue() {
      this.error = null;

      this.filteredSuggestions = this.allCoins
        .filter((coin) =>
          coin.Symbol.toLowerCase().includes(this.ticker.toLowerCase())
        )
        .map((coin) => coin.Symbol)
        .slice(0, 4);
    },

    add() {
      const upperTicker = this.ticker.toUpperCase();
      if (this.tickers.find((t) => t.name === upperTicker)) {
        this.error = "Такой тикер уже добавлен";
        return;
      }

      if (!upperTicker) {
        this.error = "Тикер не может быть пустым";
        return;
      }

      const isTickerExists = this.allCoins.find(
        (coin) => coin.Symbol.toUpperCase() === upperTicker
      );

      if (!isTickerExists) {
        this.error = "Такого тикера не существует в списке монет";
        return;
      }

      this.$emit("add-ticker", upperTicker);
      this.ticker = "";
      this.filteredSuggestions = [];
    },

    selectSuggestion(suggestion) {
      if (this.tickers.find((t) => t.name === suggestion)) {
        this.error = "Такой тикер уже добавлен";
        return;
      }

      this.$emit("add-ticker", suggestion);
    },
  },
};
</script>
