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

const currLanguage = ref('ru'); // current-language
const currYear = ref(new Date().getFullYear()); // current-year
const currMonthIndex = ref(new Date().getMonth()); // current-month-index

// language-toggler
const languageToggler = () => {
	return (currLanguage.value = currLanguage.value === 'ru' ? 'en' : 'ru');
};

// current-date-(month-&&-year)
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
	<div class="bg-white w-96 p-3 rounded-md shadow-2xl">
		<div class="flex items-center justify-between">
			<button @click="languageToggler" class="uppercase">
				{{ currLanguage }}
			</button>
			<span>{{ currMonthAndYear }}</span>
		</div>
	</div>
</template>
