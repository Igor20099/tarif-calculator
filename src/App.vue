<script setup>
import Tarif from "./components/Tarif.vue";
import { onMounted, ref } from "vue";
import axios from "axios";

const currencyKZT = ref(0);
const currencyCNY = ref(0);
const priceCurrency = ref(0);
const isShow = ref(false);
const discount = ref(0);
const currentCurrency = ref(0);
const currentPeriod = ref(0);
const activeTarif = ref(0);
const currencySymbol = ref("¥");

const tarifs = [
  { title: "Стандартный", price: { month: 100, year: 1000 } },
  { title: "Продвинутый", price: { month: 150, year: 1400 } },
];

const setData = (currency, period) => {
  currentCurrency.value = currency;
  currentPeriod.value = period;
};

const tarifCalculate = () => {
  console.log(currentPeriod.value);
  if (currentCurrency.value === 0) {
    currencySymbol.value = "¥";
    if (currentPeriod.value === 1) {
      priceCurrency.value =
        tarifs[activeTarif.value].price.year * currencyCNY.value;
      discount.value =
        tarifs[activeTarif.value].price.month * currencyCNY.value * 12 -
        priceCurrency.value;
    } else {
      priceCurrency.value =
        tarifs[activeTarif.value].price.month * currencyCNY.value;
      discount.value = 0;
    }

    console.log("Юань");
  } else if (currentCurrency.value === 1) {
    currencySymbol.value = "₸";
    if (currentPeriod.value === 1) {
      priceCurrency.value =
        tarifs[activeTarif.value].price.year * currencyKZT.value;
      discount.value =
        tarifs[activeTarif.value].price.month * currencyKZT.value * 12 -
        priceCurrency.value;
    } else {
      priceCurrency.value =
        tarifs[activeTarif.value].price.month * currencyKZT.value;
      discount.value = 0;
    }
    console.log("Тенге");
  }
  isShow.value = true;
};

onMounted(async () => {
  const data = await axios.get("https://www.cbr-xml-daily.ru/latest.js");
  console.log(data.data.rates.CNY);
  currencyKZT.value = data.data.rates.KZT;
  currencyCNY.value = data.data.rates.CNY;
});
</script>

<template>
  <h1>Тарифный калькулятор</h1>
  <h2 class="tarif-title">Тарифы</h2>
  <div class="tarif-container">
    <Tarif
      v-for="(tarif, i) in tarifs"
      :key="i"
      :tarif="tarif"
      :isActive="activeTarif === i"
      @setData="setData"
      @click="activeTarif = i"
    />
  </div>
  <div v-show="isShow">
    <p>Сумма: {{ priceCurrency }} {{ currencySymbol }}</p>
    <p>Скидка: {{ discount }} {{ currencySymbol }}</p>
  </div>
  <button @click.prevent="tarifCalculate">Рассчитать</button>
</template>

<style scoped>
.tarif-container {
  display: flex;
  flex-direction: row;
}
.tarif-title {
  font-size: 36px;
}
</style>
