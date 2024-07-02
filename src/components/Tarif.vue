<template>
  <div
    class="tarif"
    :class="props.isActive ? 'active' : ''"
    @click="$emit('setData', activeCurrency, activePeriod)"
  >
    <h2 class="title">{{ props.tarif.title }}</h2>
    <p class="price">
      {{
        (pricePeriod =
          activePeriod === 0 ? props.tarif.price.month : props.tarif.price.year)
      }}
      руб.
    </p>
    <h3 class="title-currency">Выберите валюту</h3>
    <ul class="list-currency">
      <li
        class="currency"
        v-for="(currency, i) in currencyList"
        :key="i"
        :class="activeCurrency === i ? 'active-item' : ''"
        @click="activeCurrency = i"
      >
        {{ currency }}
      </li>
    </ul>
    <h3 class="title-period">Выберите период оплаты</h3>
    <ul class="list-period">
      <li
        class="period"
        v-for="(period, i) in periodList"
        :key="i"
        :class="activePeriod === i ? 'active-item' : ''"
        @click="activePeriod = i"
      >
        {{ period }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";

const props = defineProps(["tarif", "isActive"]);
const currencyList = ["Юань,¥", "Тенге, ₸"];
const activeCurrency = ref(0);
const periodList = ["за месяц", "за год"];
const activePeriod = ref(0);
const pricePeriod = ref(0);
const emit = defineEmits(["calculate"]);
</script>

<style scoped>
.tarif {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 1px solid #fff;
  border-radius: 10px;
  padding: 24px;
  margin-right: 16px;
  cursor: pointer;
}
.title {
  font-size: 32px;
  margin: 0;
}

.price {
  font-size: 24px;
}
.list-currency {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.list-period {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.currency {
  border: 1px solid #fff;
  padding: 8px;
}
.period {
  border: 1px solid #fff;
  padding: 8px;
}
.active {
  background-color: #fff;
  color: #000;
}

.active-item {
  background-color: #000;
  color: #fff;
}
</style>
