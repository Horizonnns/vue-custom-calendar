<script setup>
import { computed, ref } from 'vue';

// monthes
const MONTHSRU = [
	'Январь',
	'Февраль',
	'Март',
	'Апрель',
	'Май',
	'Июнь',
	'Июль',
	'Август',
	'Сентябрь',
	'Октябрь',
	'Ноябрь',
	'Декабрь',
];

const MONTHSEN = [
	'January',
	'February',
	'March',
	'April',
	'May',
	'June',
	'July',
	'August',
	'September',
	'October',
	'November',
	'December',
];

const currLanguage = ref('ru');
const currYear = ref(new Date().getFullYear()); // current-year
const currMonthIndex = ref(new Date().getMonth()); // current-month-index

const currMonthAndYear = computed(() => {
	return `${
		currLanguage.value === 'ru'
			? MONTHSRU[currMonthIndex.value]
			: MONTHSEN[currMonthIndex.value]
	} ${currYear.value}`;
});

console.log('currMonthAndYear', currMonthAndYear.value);
</script>

<template>
	<span>{{ currMonthAndYear }}</span>
</template>
