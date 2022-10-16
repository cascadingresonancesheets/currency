<template>
  <div class="list__item pare" v-show="valute.CharCode !== 'RUB'">
    <p class="pare__title">
      {{ valute.Name }} |
      <span :class="classMod === 'up' ? 'pare_up' : 'pare_down'">
        {{ pareDiff }}
      </span>
    </p>
    <div class="pare__body">
      <div class="pare__content">
        <p class="pare__name">{{ pareName }}</p>
        <p class="pare__value">{{ pareValue }}</p>
      </div>
      <button class="pare__toggle" @click="togglePare"></button>
    </div>
  </div>
</template>

<script>
export default {
  name: "appPare",
  data() {
    return {
      toggled: false,
      pareName: "",
      pareValue: 0,
      pareDiff: 0,
      classMod: "",
    };
  },
  props: {
    valute: {
      type: Object,
      required: true,
    },
  },
  methods: {
    togglePare() {
      if (this.toggled) {
        this.togglePareToDefault();
      } else {
        this.pareName = "RUB / " + this.valute.CharCode;
        // prettier-ignore
        this.pareValue = "1 " + this.valute.CharCode + " - " + ((1 / this.valute.Value) * this.valute.Nominal).toFixed(4);
        this.toggled = true;
      }
    },
    togglePareToDefault() {
      this.pareName = this.valute.CharCode + " / RUB";
      // prettier-ignore
      this.pareValue = "1 RUB - " + (this.valute.Value / this.valute.Nominal).toFixed(4);
      this.toggled = false;
    },
  },
  mounted() {
    this.togglePareToDefault();
    this.pareDiff = (this.valute.Value - this.valute.Previous).toFixed(4);
    this.classMod =
      this.pareDiff > 0 ? (this.classMod = "up") : (this.classMod = "down");
  },
};
</script>

<style lang="scss" scoped>
.pare {
  flex-basis: calc(50% - 12px);
  background-color: #f4f4f4;
  padding: 24px 18px;

  &_up {
    color: #16a452;
    &::before {
      content: "+";
    }
    &::after {
      content: "";
      position: relative;
      left: 6px;
      top: -1px;
      border: solid #16a452;
      border-width: 0 2px 2px 0;
      display: inline-block;
      padding: 2px;
      transform: rotate(-135deg);
    }
  }

  &_down {
    color: #e12c2c;
    &::after {
      content: "";
      position: relative;
      left: 6px;
      top: -3px;
      border: solid #e12c2c;
      border-width: 0 2px 2px 0;
      display: inline-block;
      padding: 2px;
      transform: rotate(45deg);
    }
  }

  &__body {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__title {
    padding-bottom: 12px;
    line-height: 24px;
  }

  &__name {
    padding-bottom: 5px;
  }

  &__value {
    font-weight: bold;
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
