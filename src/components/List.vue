<template>
  <input class="search" type="text" v-model="searchQuery" placeholder="Поиск" />
  <div class="list">
    <app-pare v-for="item in searchedValutes" :valute="item" :key="item.ID" />
  </div>
</template>

<script>
import AppPare from "@/components/Pare";

export default {
  components: {
    AppPare,
  },
  name: "appList",
  props: {
    valutes: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      searchQuery: "",
    };
  },
  computed: {
    searchedValutes() {
      return this.valutes.filter(
        (valute) =>
          valute.Name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          valute.CharCode.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
};
</script>

<style lang="scss" scoped>
.search {
  height: 45px;
  width: 100%;
  border-radius: 2px;
  background: #f4f4f4;
  border: 1px solid #dcdcdc;
  padding: 10px;
  font-size: 16px;
  color: #202020;
  margin-bottom: 24px;
  font-family: "Roboto Condensed";

  &::placeholder {
    color: #7a7a7a;
  }
}

.list {
  display: flex;
  gap: 24px;
  flex-wrap: wrap;
}
</style>
