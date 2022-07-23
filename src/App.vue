<script setup>
import { ref, computed } from "vue";

const flightType = ref("one");
const departureDate = ref(dateToString(new Date()));
const returnDate = ref(departureDate.value);

const canBook = computed(
  () =>
    !isReturn.value ||
    stringToDate(returnDate.value) > stringToDate(departureDate.value)
);
const isReturn = computed(() => flightType.value === "return");
function dateToString(date) {
  return (
    date.getFullYear() +
    "-" +
    pad(date.getMonth() + 1) +
    "-" +
    pad(date.getDate())
  );
}

function stringToDate(str) {
  const [y, m, d] = str.split("-");
  return new Date(+y, m - 1, +d);
}

function pad(n, s = String(n)) {
  return s.length < 2 ? `0${s}` : s;
}

function handleBook() {
  console.log(departureDate.value, returnDate.value);
}
</script>

<template>
  <select v-model="flightType">
    <option value="one">One-way Flight</option>
    <option value="return">Return Flight</option>
  </select>

  <input type="date" v-model="departureDate" />
  <input type="date" :disabled="!isReturn" v-model="returnDate" />

  <button @click="handleBook" :disabled="!canBook">Book</button>
</template>

<style>
select,
input,
button {
  display: block;
  margin: 0.5em, 0;
  font-size: 15px;
}
input[disabled] {
  color: #999;
}
</style>
