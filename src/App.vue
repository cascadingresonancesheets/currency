<template>
  <div class="app-tabs" @click="selectTab">
    <div
      class="app-tabs__btn"
      :class="{ 'app-tabs__btn_active': selectedTab === 'list' }"
      data-tab="list"
    >
      Список валют
    </div>
    <div
      class="app-tabs__btn"
      :class="{ 'app-tabs__btn_active': selectedTab === 'converter' }"
      data-tab="converter"
    >
      Конвертер
    </div>
  </div>
  <div class="tab-content" v-if="selectedTab === 'list'">
    <app-list :valutes="valutes" />
  </div>
  <div class="tab-content" v-if="selectedTab === 'converter'">
    <app-converter :valutes="valutes" />
  </div>
</template>

<script>
import axios from "axios";
import AppList from "@/components/List";
import AppConverter from "@/components/Converter";

export default {
  components: {
    AppList,
    AppConverter,
  },
  data() {
    return {
      valutes: [],
      selectedTab: "list",
    };
  },
  methods: {
    selectTab(e) {
      if (e.target.dataset.tab) this.selectedTab = e.target.dataset.tab;
    },
    async fetchValutes() {
      try {
        const response = await axios.get(
          "https://www.cbr-xml-daily.ru/daily_json.js"
        );

        // После получения переводим в массив и добавляем базовый RUB = 1
        this.valutes = Object.values(response.data.Valute);
        this.valutes.unshift({
          ID: "RUB",
          CharCode: "RUB",
          Nominal: 1,
          Name: "Российский рубль",
          Value: 1,
          Previous: 1,
        });
      } catch (e) {
        console.log(e);
      }
    },
  },
  mounted() {
    this.fetchValutes();
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@400;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  overflow-y: scroll;
  background-color: #e6e6e6;
  font-family: "Roboto Condensed", sans-serif;
  color: #202020;
}

#app {
  max-width: 732px;
  margin: 200px auto 0;
}

.app-tabs {
  display: flex;
  user-select: none;

  &__btn {
    background-color: #f0f0f0;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;

    &_active {
      background-color: #fff;
    }

    &:first-child {
      border-radius: 3px 0 0 0;
    }

    &:last-child {
      border-radius: 0 3px 0 0;
    }
  }
}

.tab-content {
  background-color: #fff;
  min-height: 64px;
  padding: 24px;
  padding-top: 42px;
}
</style>
