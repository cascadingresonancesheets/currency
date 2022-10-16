<template>
  <div class="converter">
    <div class="converter-form">
      <select
        class="converter-form__select"
        v-model="baseValuteSelected"
        @change="updateValuteLists"
      >
        <option v-for="v in baseValutesList" :value="v.CharCode" :key="v.ID">
          {{ v.CharCode }}
        </option>
      </select>
      <input
        class="converter-form__input"
        type="number"
        v-model="baseInputValue"
        @input="convert"
      />
    </div>

    <button class="converter-form__toggle" @click="toggleValutes"></button>

    <div class="converter-form">
      <select
        class="converter-form__select"
        v-model="calcValuteSelected"
        @change="updateValuteLists"
      >
        <option v-for="v in calcValutesList" :value="v.CharCode" :key="v.ID">
          {{ v.CharCode }}
        </option>
      </select>
      <input
        class="converter-form__input"
        type="text"
        disabled
        v-model="calcInputValue"
        @input="convert"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "appConverter",
  props: {
    valutes: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      baseValutesList: [],
      baseValuteSelected: "USD",
      baseInputValue: 1,

      calcValutesList: [],
      calcValuteSelected: "BYN",
      calcInputValue: 0,
    };
  },
  methods: {
    updateBaseList() {
      this.baseValutesList = this.valutes.filter(
        (item) => item.CharCode !== this.calcValuteSelected
      );
    },
    updateCalcList() {
      this.calcValutesList = this.valutes.filter(
        (item) => item.CharCode !== this.baseValuteSelected
      );
    },
    updateValuteLists() {
      this.updateBaseList();
      this.updateCalcList();
      this.convert();
    },
    convert() {
      const baseIndex = this.valutes.filter((item) =>
        item.CharCode.includes(this.baseValuteSelected)
      )[0];
      const calcIndex = this.valutes.filter((item) =>
        item.CharCode.includes(this.calcValuteSelected)
      )[0];
      // prettier-ignore
      this.calcInputValue = ( ( (baseIndex.Value / baseIndex.Nominal) / (calcIndex.Value / calcIndex.Nominal) ) * this.baseInputValue ).toFixed(2);
    },
    toggleValutes() {
      const temp = this.baseValuteSelected;
      this.baseValuteSelected = this.calcValuteSelected;
      this.calcValuteSelected = temp;
      this.baseInputValue = this.calcInputValue;
      this.updateValuteLists();
      this.convert();
    },
  },
  mounted() {
    this.updateValuteLists();
  },
};
</script>

<style lang="scss" scoped>
.converter {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.converter-form {
  display: flex;
  gap: 20px;

  &__select,
  &__input {
    background: #ffffff;
    border: 1px solid #555555;
    border-radius: 3px;
    height: 45px;
    padding: 10px;
    font-family: "Roboto Condensed";
    font-size: 16px;
    color: #202020;
  }

  &__input {
    min-width: 207px;

    &:disabled {
      background: #f4f4f4;
      border: 1px solid #dcdcdc;
    }
  }

  &__toggle {
    width: 36px;
    height: 36px;
    background-image: url("../../public/toggle.png");
    background-repeat: no-repeat;
    background-position: center;
    background-color: transparent;
    border: none;
    cursor: pointer;
  }
}
</style>
