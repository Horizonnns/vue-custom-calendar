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

// current-days-of-month-in-array
const currDate = computed(() => {
	let date = new Date(currYear.value, currMonthIndex.value + 1, 0);
	return (date = Array.from({ length: date.getDate() }, (_, i) => i + 1));
});

// first-day-index-of-current-month
const firstDayIndex = computed(() => {
	return new Date(currYear.value, currMonthIndex.value, 1).getDay() - 1;
});

// current-days-of-month
const daysToShow = computed(() => {
	const days = [];

	for (let i = 0; i < firstDayIndex.value; i++) {
		days.push({ index: i, empty: true });
	}

	currDate.value.forEach((day) => {
		days.push({ number: day, empty: false });
	});

	return days;
});
</script>

<template>
	<div class="bg-white w-96 p-3 rounded-md shadow-2xl space-y-2">
		<!-- header -->
		<div class="flex items-center justify-between">
			<button @click="languageToggler" class="uppercase">
				{{ currLanguage }}
			</button>
			<span>{{ currMonthAndYear }}</span>
		</div>

		<!-- calendar-body -->
					<button
						:class="day.empty ? 'bg-gray-200' : ''"
						class="border w-9 h-9"
					>
						{{ day.number }}
					</button>
			</div>
		</div>
	</div>
</template>
